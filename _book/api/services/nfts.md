# NFT Service

The `NFTService` provides high-level methods to discover and interact with NFT assets on the Elacity platform. It handles both standard ERC-721 assets and Protected/DRM-enabled assets.

## Fetching NFT Items

Use `fetchItems` to retrieve a paginated list of NFTs. You can filter by category, collection, or variant.

```typescript
const result = await client.nfts.fetchItems(
  {
    category: ['Art', 'Music'],
    variant: 'Standard'
  },
  {
    limit: 10,
    offset: 0,
    sortBy: 'createdAt'
  }
);

console.log('Total items:', result.fetchNFTItems.total);
console.log('Items:', result.fetchNFTItems.data);
```

Authentication: **Optional**

### Filtering Options

The `query` parameter supports:
- `category`: Array of strings.
- `collectionAddresses`: Array of contract addresses.
- `address`: Specific owner or contract address.
- `withUnpublished`: Boolean (requires authentication).

## Retrieving a Single Item

To get detailed information about a specific NFT:

```typescript
const item = await client.nfts.retrieveItem(contractAddress, tokenId);

console.log('Name:', item.retrieveNFTItem.name);
console.log('Metadata:', item.retrieveNFTItem.metadata);
```

Authentication: **Optional**

## Protected Assets

For assets with DRM (Digital Rights Management), the response includes an `operative` field and `access` details:

```typescript
if (item.retrieveNFTItem.__typename === 'ProtectedAsset') {
  console.log('Access Level:', item.retrieveNFTItem.access);
  console.log('Operative Contract:', item.retrieveNFTItem.operative.address);
}
```
