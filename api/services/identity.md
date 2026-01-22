# Identity Service

The `IdentityService` provides a federated view of a user's presence on the Elacity platform, combining their wallet address, Smart Account, and Decentralized Identity (DID).

## Federated Account

The `Account` object is the central entity for identity. It includes profile metadata, social links, and sub-identities.

```typescript
export interface Account {
  address: string;      // Primary wallet address
  alias?: string;       // Username/Alias
  email?: string;       // Linked email
  avatar?: string;      // Profile picture
  sa?: string;          // Elastos Smart Account address
  did?: DIdentity;     // Decentralized Identity details
  isAdmin?: boolean;    // Admin status
}
```

## Retrieving Identity

### Authenticated User (`me`)

Retrieves the full profile for the currently logged-in user.

```typescript
const profile = await client.identity.me();
console.log(`Welcome, ${profile.alias}`);
console.log(`Smart Account: ${profile.sa}`);
```

### Discovery

Search for other users or retrieve specific profiles.

```typescript
// Fetch by address
const user = await client.identity.retrieveAccount({
  address: '0x123...'
});

// Search users
const result = await client.identity.fetchAccounts({
  alias: 'Elacity'
});
```

## Social Interactions

Manage following relationships and discover social connections.

### Following / Unfollowing

```typescript
// Follow a user or channel
await client.identity.follow('0x...');

// Unfollow
await client.identity.unfollow('0x...');
```

### Followers List

```typescript
const result = await client.identity.listSubscribers('0x...');
console.log(`Followers count: ${result.count}`);
```
