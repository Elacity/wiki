# Core Reference

This section documents `@elacity-js/core`, the shared foundation package used across Elacity SDKs.
It contains small, reusable primitives (chains, auth types/storage interfaces, and pagination shapes) that other packages depend on.

## What it contains

### Chains

- `ChainId`: an enum of supported chain IDs used consistently across SDKs (API + contracts).
- See: [Chains](Chains.md)

### Authentication primitives

- `AuthUser`: shape of an authenticated session (address, token, expiry, optional smart-account address).
- `AuthTokenStorage`: storage interface to persist/load/clear a session.
- `MemoryTokenStorage`: in-memory `AuthTokenStorage` implementation.
- `AuthSigner`: minimal interface required for “sign-in with wallet” style flows.
- See: [Authentication](Authentication.md)

### Pagination primitives

- `FilterPaginationInput`: common query options (offset/limit/sort/search).
- `PaginatedResponse<T>`: common paginated response shape.
- See: [Pagination](Pagination.md)


## References

* [Authentication](Authentication.md)
* [Pagination](Pagination.md)
* [Chains](Chains.md)
