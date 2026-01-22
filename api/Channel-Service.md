# Channel Service

The `ChannelService` provides methods for discovering, creating, and managing Elacity Channels, including subscription plans and access control.

## Discovery

### Fetching Channels

```typescript
const result = await client.channels.fetchChannels({
  categories: ['Music']
});
```

### Retrieving Channel Details

```typescript
const channel = await client.channels.retrieveChannel({
  address: '0x...'
});
```

## Management (Authenticated)

Most management methods require the user to be logged in.

### Creating a Channel

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

### Updating Channel Info

```typescript
await client.channels.updateChannelInformation('0x...', {
  description: "Updated description for my channel",
  categories: ["Art", "Lifestyle"]
});
```

### Managing Subscription Plans

You can add, update, or remove subscription tiers using the `updateSubscriptionPlan` method.

```typescript
await client.channels.updateSubscriptionPlan('0x...', [
  {
    action: 'ADD',
    args: {
      label: "Premium Tier",
      duration: { value: 365, unit: "days" },
      price: "100",
      payToken: "0x..."
    }
  }
]);
```
