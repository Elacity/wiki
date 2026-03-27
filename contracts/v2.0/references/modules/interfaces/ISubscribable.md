## ISubscribable

Minimal subscription primitives for contracts that sell access plans.

### NotSubscribed

```solidity
error NotSubscribed(uint8 planId, address subscriber)
```

Thrown when a subscription-dependent action is requested without an active plan.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| planId | uint8 | Identification of the subscription plan |
| subscriber | address | Address of the subscriber |

### ActiveSubscriptionNotExpired

```solidity
error ActiveSubscriptionNotExpired(address subscriber)
```

Thrown when trying to resubscribe before the active subscription expires.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| subscriber | address | Address of the subscriber |

### Subscription

Canonical subscription record stored for each subscriber.

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

Emitted when an account subscribes to a plan.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| subscriber | address | Subscriber address. |
| planId | uint8 | Plan identifier. |
| expiry | uint256 | Subscription expiration timestamp. |
| recurring | bool | Recurring flag for the subscription. |

### AccountUnsubscribed

```solidity
event AccountUnsubscribed(address subscriber, uint8 planId)
```

Emitted when an account unsubscribes from a plan.

### subscribePlan

```solidity
function subscribePlan(uint8 planId, bool recurring) external payable
```

Subscribes `msg.sender` to a plan.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| planId | uint8 | Plan identifier. |
| recurring | bool | Whether the subscription should be recurring. |

### hasActiveSubscription

```solidity
function hasActiveSubscription(address subscriber) external view returns (bool)
```

Returns whether an account currently has an active subscription.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| subscriber | address | Account to evaluate. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bool | `true` if the account has a non-expired subscription. |

