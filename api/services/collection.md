# Collection Service

The `CollectionService` allows you to discover and retrieve detailed information about NFT collections tracked by the Elacity platform.

## Fetching Collections

Use `fetchCollections` to search for collections based on names, addresses, or status.

```typescript
const result = await client.collections.fetchCollections({
  collectionName: 'Elacity',
  status: true
}, {
  limit: 10,
  offset: 0
});

console.log(`Found ${result.total} collections`);
result.data.forEach(col => {
  console.log(`- ${col.collectionName} (${col.erc721Address})`);
});
```

### Query Criteria

| Field | Type | Description |
|-------|------|-------------|
| `erc721Address` | `string` | The contract address of the collection. |
| `collectionName` | `string` | Search by collection name (partial match). |
| `isAppropriate` | `boolean` | Filter by moderation status. |
| `status` | `boolean` | Filter by active/inactive status. |

## Retrieving a Specific Collection

Use `retrieveCollection` when you know the specific address or name of the collection you want details for.

```typescript
const collection = await client.collections.retrieveCollection({
  erc721Address: '0x123...'
});

console.log(collection.description);
console.log(`Royalty: ${collection.royalty}%`);
```

Authentication: **Optional**
