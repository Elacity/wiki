# Authentication Types

The `@elacity-js/common` package defines the fundamental interfaces and types for authentication used across the Elacity SDK ecosystem.

## AuthUser

Represents an authenticated user session returned by the backend.

| Property | Type | Description |
| :--- | :--- | :--- |
| `address` | `string` | The wallet address (EOA) that performed the login. |
| `token` | `string` | The JWT access token for authenticating subsequent API requests. |
| `expiresIn` | `number` | Token expiration time in seconds from the moment of issue. |
| `sa` | `string` | (Optional) The address of the Smart Account associated with this session. |

```typescript
export interface AuthUser {
  address: string;
  token: string;
  expiresIn: number;
  sa?: string;
}
```

## AuthAccount

A derived entity representing the operational account for the session.

| Property | Type | Description |
| :--- | :--- | :--- |
| `address` | `string` | The active account address. This is either the `sa` (Smart Account) if present, or the `address` (EOA). |
| `owner` | `string` | The EOA which owns/controls the account. Always the login wallet address. |

```typescript
export interface AuthAccount {
  address: string;
  owner: string;
}
```

## AuthTokenStorage

An interface for persisting authentication sessions. You can implement this to store tokens in `localStorage`, cookies, or a database.

### Methods

#### `load(): AuthUser | null`
Loads the stored session. Returns `null` if no session exists or it has expired.

#### `save(user: AuthUser): void`
Persists the provides [AuthUser](#authuser) session.

#### `clear(): void`
Removes the session from storage.

```typescript
export interface AuthTokenStorage {
  load(): AuthUser | null;
  save(user: AuthUser): void;
  clear(): void;
}
```

### MemoryTokenStorage

A built-in implementation that stores the token in memory (non-persistent). Suitable for server-side usage or test environments.

```typescript
import { MemoryTokenStorage } from '@elacity-js/common';

const storage = new MemoryTokenStorage();
```

## AuthSigner

An interface for wallet signing implementations, compatible with `ethers.Signer` and `viem` accounts.

### Methods

#### `getAddress(): Promise<string>`
Returns a promise that resolves to the wallet address of the signer.

#### `signMessage(message: string): Promise<string>`
Signs a plaintext message and returns the signature string.

```typescript
export interface AuthSigner {
  getAddress(): Promise<string>;
  signMessage(message: string): Promise<string>;
}
```
