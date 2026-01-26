## ISubscribable

Provides the minimal requriments for a subscribeable NFT contract

### NotSubscribed

```solidity
error NotSubscribed(uint8 planId, address subscriber)
```

Error thrown when the user have no active subscription

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| planId | uint8 | Identification of the subscription plan |
| subscriber | address | Address of the subscriber |

### ActiveSubscriptionNotExpired

```solidity
error ActiveSubscriptionNotExpired(address subscriber)
```

Error thrown when a user still have an active subscription

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| subscriber | address | Address of the subscriber |

### Subscription

We hold subscription data on top of this format to keep on track of expiration
date and check whether a user has active subscription

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

```solidity
struct Subscription {
  uint8 planId;
  address subscriber;
  uint256 timestamp;
  uint256 expiry;
  bool recurring;
}
```

### AccountSubscibed

```solidity
event AccountSubscibed(address subscriber, uint8 planId, uint256 expiry, bool recurring)
```

This event signals a user have been subscribed to a specific plan

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| subscriber | address | The subscriber address |
| planId | uint8 | The identification of the subscription plan |
| expiry | uint256 | The date the subscription will end |
| recurring | bool | Flag of recurring state of the subscription |

### AccountUnsubscribed

```solidity
event AccountUnsubscribed(address subscriber, uint8 planId)
```

### subscribePlan

```solidity
function subscribePlan(uint8 planId, bool recurring) external payable
```

Subscribe a user to a specific subscription plan

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| planId | uint8 | The identification of the subscription plan |
| recurring | bool | Flag of recurring state of the subscription |

### hasActiveSubscription

```solidity
function hasActiveSubscription(address subscriber) external view returns (bool)
```

Check whether an account has an active subscrition

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| subscriber | address | Address of the subscriber to check |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bool | bool |

