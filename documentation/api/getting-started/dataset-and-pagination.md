# Dataset and Pagination

The `@elacity-js/api` SDK communicates with the Elacity backend through two transports:

- **GraphQL** (`/api/2.0/graphql`) — used for most data queries and mutations.
- **REST** (`/api/*`) — used for specific operations such as collection updates and file uploads.

The `ElacityClient` abstracts both transports behind a single, developer-friendly interface. Each service method (e.g. `client.nfts.fetchItems(...)`) internally selects the appropriate transport, so you never need to construct raw HTTP or GraphQL requests yourself.

## GraphQL Data Layer

All list queries go through the GraphQL endpoint at `/api/2.0/graphql`. The schema exposes a rich set of queries and mutations covering NFTs, channels, collections, accounts, subscriptions, and more.

### Custom Scalars

The GraphQL schema defines several custom scalar types you may encounter in responses:

| Scalar | Description |
| :--- | :--- |
| `JSON` | Arbitrary JSON values |
| `BigNumber` | Large numeric values (e.g. token balances, prices in wei) |
| `TokenID` | Token identifier (may be numeric or hex-encoded) |
| `Date` | ISO date/time values |

## Paginated Queries

Most list-based queries (e.g. fetching NFTs, channels, collections, accounts) follow a consistent pagination pattern.

### Request: `FilterPaginationInput`

All paginated service methods accept an options object matching the `FilterPaginationInput` interface from `@elacity-js/common`:

```typescript
import { FilterPaginationInput } from '@elacity-js/common';

const options: FilterPaginationInput = {
  offset: 0,       // Number of items to skip (default: 0)
  limit: 20,       // Page size (default: 20)
  sortBy: 'createdAt',  // Field to sort by
  sort: { createdAt: -1 },  // Sort direction: 1 = ascending, -1 = descending
  searchBy: 'keyword',  // Optional text search
};
```

| Field | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `offset` | `number` | `0` | Number of records to skip. |
| `limit` | `number` | `20` | Maximum number of records to return. |
| `sortBy` | `string` | `'createdAt'` | Field name to sort by. |
| `sort` | `Record<string, number>` | — | MongoDB-style sort object, e.g. `{ createdAt: -1 }`. |
| `searchBy` | `string` | — | Free-text search filter. |

### Response: `PaginatedResponse<T>`

Paginated queries return a `PaginatedResponse<T>` object:

```typescript
interface PaginatedResponse<T> {
  total: number;   // Total matching records on the server
  offset: number;  // Current offset (echoed back)
  limit: number;   // Page size (echoed back)
  data: T[];       // Array of items for this page
}
```

### Usage Example

```typescript
import { ElacityClient } from '@elacity-js/api';

const client = new ElacityClient();

// Fetch the first page of NFTs
const page1 = await client.nfts.fetchItems(
  { variant: 'drm' },       // query filter
  { offset: 0, limit: 10 }  // pagination options
);

console.log(page1.total);  // e.g. 243 (total matching items)
console.log(page1.data);   // NFTItem[] (up to 10 items)
console.log(page1.offset); // 0
console.log(page1.limit);  // 10

// Fetch the next page
const page2 = await client.nfts.fetchItems(
  { variant: 'drm' },
  { offset: 10, limit: 10 }
);
```

### Iterating Through All Pages

To retrieve all records across multiple pages:

```typescript
async function fetchAll<T>(
  fetcher: (options: FilterPaginationInput) => Promise<PaginatedResponse<T>>,
  pageSize = 20
): Promise<T[]> {
  const all: T[] = [];
  let offset = 0;
  let total = Infinity;

  while (offset < total) {
    const page = await fetcher({ offset, limit: pageSize });
    total = page.total;
    all.push(...page.data);
    offset += pageSize;
  }

  return all;
}

// Example: fetch all channels
const allChannels = await fetchAll(
  (opts) => client.channels.fetchChannels({}, opts)
);
```

> [!NOTE]
> Use pagination wisely. Fetching all records at once may impact performance on large datasets. Prefer paginated access with reasonable `limit` values.

## Sorting

You can control the sort order using either `sortBy` (simple field name) or `sort` (object with direction):

```typescript
// Sort by newest first
const newest = await client.nfts.fetchItems({}, {
  sortBy: 'createdAt',
  // OR
  sort: { createdAt: -1 }
});

// Sort by price ascending
const cheapest = await client.nfts.fetchItems({}, {
  sort: { price: 1 }
});
```

## Common Response Types

The following paginated response types are used across services:

| Service | Method | Response Type |
| :--- | :--- | :--- |
| NFTs | `fetchItems()` | `PaginatedResponse<NFTItem>` |
| Channels | `fetchChannels()` | `PaginatedResponse<Channel>` |
| Identity | `fetchAccounts()` | `PaginatedResponse<Account>` |
| Identity | `fetchSubscriptions()` | `PaginatedResponse<Subscription>` |
| Background Jobs | `fetchBackgroundJobs()` | `PaginatedResponse<BackgroundJob>` |

Each `NFTItem` in the response is a GraphQL union type that resolves to either a `StandardAsset` or a `ProtectedAsset`, depending on the token variant.
