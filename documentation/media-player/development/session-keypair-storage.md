# Session Keypair Storage

since `v0.10.x`

---


The player can persist the ECDH/Ed25519 session keypair across page reloads to avoid re-signing on every playback. This is controlled by the `session` option in player setup:

```js
setup({
  session: {
    expiresAt: Math.floor(Date.now() / 1000) + 3600, // Unix timestamp
  },
  // ...
})
```

When `session` is `null` or `undefined` (the default), no keypair is persisted and a fresh one is generated on every load.

## Implementation — Option A (OPFS)

The 32-byte Ed25519 private seed (from which both the Ed25519 signing key and the X25519 ECDH key are derived) is written to a binary file in the browser's [Origin Private File System](https://developer.mozilla.org/en-US/docs/Web/API/File_System_API/Origin_private_file_system) via Emscripten's filesystem layer.

File layout (`/session/keypair.bin`):

| Offset | Size | Content |
|---|---|---|
| 0 | 8 | Expiry timestamp (u64 big-endian, Unix seconds) |
| 8 | 32 | Ed25519 private seed (raw bytes) |

On init, the player checks whether the file exists and the timestamp has not passed. If valid, it rebuilds the keypair from the stored seed instead of generating a new one. On first generation (or after expiry), it writes the new seed and expiry to the file.

## Known Security Limitations

> **This section documents deliberate design trade-offs. Read before shipping to production.**

### What OPFS provides

- **Origin isolation**: the file is only accessible to pages served from the same origin. Cross-origin scripts and other sites cannot read it.
- **OS-level encryption at rest**: on most platforms, browser profile storage is encrypted by the OS (full-disk encryption, or per-user encryption on macOS/Windows).

### What OPFS does NOT provide

#### Same-origin XSS — full seed exposure

An attacker who achieves JavaScript execution on the same origin (XSS, compromised CDN script, malicious browser extension with host permissions) can read the raw seed bytes directly:

```js
const root = await navigator.storage.getDirectory();
const fh = await root.getFileHandle('keypair.bin');
const file = await fh.getFile();
const seed = new Uint8Array(await file.arrayBuffer()).slice(8, 40); // 32-byte seed
```

The seed can then be exfiltrated and used permanently from any machine to forge valid license request signatures on behalf of this user.

#### Local device / filesystem forensics

OPFS files are stored in the browser's profile directory as unencrypted bytes (Chrome: `Profile/File System/`, Firefox: `storage/default/`). Anyone with read access to the filesystem (local attacker, malware, stolen device without full-disk encryption) can read the file directly without any browser credential.

#### WASM heap exposure during restore

When the seed is loaded from OPFS to rebuild the keypair, it passes through WASM linear memory (`HEAPU8`) for the duration of `_restore_session_keys`. A process-level memory dump during this window can capture the seed.

### Threat model summary

| Attacker | Can extract seed? |
|---|---|
| Remote (no JS execution, no device access) | No |
| Same-origin XSS | **Yes — full exfiltration** |
| Filesystem access (no browser running) | **Yes — raw bytes on disk** |
| Filesystem access (DPAPI/Keychain protected OS) | Harder, depends on OS encryption |
| Browser process memory dump | Yes, during brief restore window |
| Different-origin script | No (origin isolation) |

### Accepted risk

This storage strategy is appropriate for a **soft session token** whose only capability is signing license requests for content the user already has access to. The consequence of key theft is that an attacker can make license requests as this user — not that they gain access to the content itself (which is separately governed by on-chain access checks).

For a higher security bar, see the [Session Keypair Storage — Security Alternatives](#) section (not yet implemented). The relevant options evaluated were:

- **Non-extractable WebCrypto `CryptoKey` in IndexedDB** (Option B): private key never exists as extractable bytes; XSS is limited to a signing oracle, not full key exfiltration. Requires keypair generation to move to the JS side and signing to route through `crypto.subtle.sign`, which conflicts with the current C-side `_eddsa_sign_message` architecture and introduces pthread-to-main-thread async complexity.
- **OPFS ciphertext + non-extractable AES key in IndexedDB** (combined A+B): seed is AES-256-GCM encrypted at rest; disk forensics without DPAPI/Keychain yields only ciphertext. XSS still has a decrypt oracle (`subtle.decrypt` is callable with a non-extractable key), so full seed exfiltration via XSS remains possible. Better than plain Option A for at-rest protection.

Option A was chosen for its simplicity and minimal C/JS changes. The security limitations above should be revisited if the session key's scope expands beyond license signing.
