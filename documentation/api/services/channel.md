# Channel Service

The `ChannelService` provides methods for discovering, creating, and managing Elacity Channels, including subscription plans and access control.

## Methods

### Discovery

#### `fetchChannels(query?, options?): Promise<FetchChannelsResponse>`

Fetches a paginated list of channels based on filtering criteria.

**Parameters:**
| Parameter | Type | Description |
| :--- | :--- | :--- |
| `query` | `ChannelQueryInput` | Filtering criteria (categories, creator, address, etc.) |
| `options` | `FilterPaginationInput` | [Pagination options](../../common/Pagination.md) |

**Returns:** `Promise<FetchChannelsResponse>` - A paginated list of `Channel` objects.

```typescript
const result = await client.channels.fetchChannels({
  categories: ['Music']
});
```

Authentication: **Optional**

#### `retrieveChannel(query): Promise<Channel>`

Retrieves detailed information for a specific channel.

**Parameters:**
- `query`: `ChannelQueryInput` - Must contain at least `address` or `_id`.

**Returns:** `Promise<Channel>`

```typescript
const channel = await client.channels.retrieveChannel({
  address: '0x...'
});
```

Authentication: **Optional**

#### `fetchUserChannels(options?): Promise<FetchChannelsResponse>`

Fetches a paginated list of channels created by the currently authenticated user.

**Parameters:**
- `options`: `FilterPaginationInput` - [Pagination options](../../common/Pagination.md)

**Returns:** `Promise<FetchChannelsResponse>`

Authentication: **Required**

#### `fetchMintableChannels(options?): Promise<FetchChannelsResponse>`

Fetches a paginated list of channels where the authenticated user has minting permissions.

**Parameters:**
- `options`: `FilterPaginationInput` - [Pagination options](../../common/Pagination.md)

**Returns:** `Promise<FetchChannelsResponse>`

Authentication: **Required**

---

### Management

#### `createChannel(input): Promise<Channel>`

Creates a new channel on the platform.

**Parameters:**
| Parameter | Type | Description |
| :--- | :--- | :--- |
| `input` | `ChannelInput` | Complete profile for the new channel. |

**Returns:** `Promise<Channel>`

```typescript
const newChannel = await client.channels.createChannel({
  name: "My Creator Channel",
  address: "0x...",
  creator: "0x...",
  channelType: "1",
  scope: "public",
  image: "ipfs://...",
  coverImage: "ipfs://...",
  plans: [
    {
      label: "Basic Access",
      duration: { value: 30, unit: "days" },
      price: "10",
      payToken: "0x..."
    }
  ]
});
```

Authentication: **Required**

#### `updateChannelInformation(address, input): Promise<Channel>`

Updates the profile of an existing channel.

**Parameters:**
| Parameter | Type | Description |
| :--- | :--- | :--- |
| `address` | `string` | The contract address of the channel. |
| `input` | `ChannelInformationInput` | Updated metadata fields. |

**Returns:** `Promise<Channel>`

Authentication: **Required**

#### `updateSubscriptionPlan(address, actions): Promise<Channel>`

Manages subscription tiers for a channel.

**Parameters:**
| Parameter | Type | Description |
| :--- | :--- | :--- |
| `address` | `string` | The contract address of the channel. |
| `actions` | `SubscriptionPlanUpdateAction[]` | List of plan updates (ADD, UPDATE, REMOVE). |

**Returns:** `Promise<Channel>`

Authentication: **Required**

## Types

### Channel
```typescript
interface Channel {
  _id: string;
  name: string;
  address: string;
  description?: string;
  channelType: string;
  imageURL: string;
  coverImageURL: string;
  itemsCount: number;
}
```

### ChannelInput
| Property | Type | Description |
| :--- | :--- | :--- |
| `name` | `string` | Display name for the channel. |
| `address` | `string` | Deploy contract address. |
| `creator` | `string` | Owner wallet address. |
| `channelType` | `string` | Category ID. |
| `scope` | `string` | visibility ('public' or 'private'). |
| `plans` | `SubscriptionPlanInput[]` | List of Tiers. |
