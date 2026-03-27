# Central Storage

`CentralStorage` is the protocol storage hub in V3.

## What it contains

- system acknowledgment/role tracking,
- IP/content tracking,
- marketplace listing/offer tracking,
- channel wrapper registry,
- fee/protocol-share configuration.

## Typical usage

Client applications mostly use this as a read/query source while write paths are handled by authorized protocol contracts.

## Migration note

Use `CentralStorage` instead of legacy `CoreStorage`.
