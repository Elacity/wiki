# Authentication Types

The `@elacity-js/common` package defines the fundamental interfaces and types for authentication used across the Elacity SDK ecosystem.

## AuthUser

Represents an authenticated user session.

```typescript
export interface AuthUser {
  address: string;  // Wallet address
  token: string;    // JWT access token
  expiresIn: number; // Token expiration in seconds
  sa?: string;      // Optional Smart Account address
}
```

## AuthTokenStorage

An interface for persisting authentication sessions. You can implement this to store tokens in `localStorage`, cookies, or a database.

```typescript
export interface AuthTokenStorage {
  load(): AuthUser | null;
  save(user: AuthUser): void;
  clear(): void;
}
```

### MemoryTokenStorage

A built-in implementation that stores the token in memory (non-persistent).

```typescript
import { MemoryTokenStorage } from '@elacity-js/common';

const storage = new MemoryTokenStorage();
```

## AuthSigner

An interface for wallet signing, compatible with `ethers.Signer`.

```typescript
export interface AuthSigner {
  getAddress(): Promise<string>;
  signMessage(message: string): Promise<string>;
}
```
