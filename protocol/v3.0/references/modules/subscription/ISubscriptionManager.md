# ISubscriptionManager
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/modules/subscription/ISubscriptionManager.sol)

**Inherits:**
[ISubscriptionPrimitive](/contracts/modules/subscription/ISubscriptionPrimitive.md)

**Title:**
ISubscriptionManager

Channel-scoped subscription manager used by channel contracts as a singleton backend.


## Functions
### configureChannel

Configure initial plans for a channel.

Callable only by `channel` itself.


```solidity
function configureChannel(address channel, SubscriptionPlan[] calldata plans) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Channel whose plans are being initialized.|
|`plans`|`SubscriptionPlan[]`|Initial plan set.|


### nextPlanId

Returns the next plan id for a channel.


```solidity
function nextPlanId(address channel) external view returns (uint8);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|Next monotonic plan id.|


### getPlan

Returns the plan by id for a channel.


```solidity
function getPlan(address channel, uint8 planId) external view returns (SubscriptionPlan memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|
|`planId`|`uint8`|Plan identifier.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`SubscriptionPlan`|Plan descriptor.|


### getPlans

Returns all plans for a channel.


```solidity
function getPlans(address channel) external view returns (SubscriptionPlan[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`SubscriptionPlan[]`|plans Full plan array from `1..lastPlanId`.|


### bulkUpdatePlans

Bulk plan updates for a channel, authorized by `actor`.

Callable only by `channel` itself.


```solidity
function bulkUpdatePlans(address channel, UpdateAction[] calldata actions, address actor) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|
|`actions`|`UpdateAction[]`|Mutation actions.|
|`actor`|`address`|Account being authorized as channel plan manager.|


### subscribePlan

Subscribes `subscriber` to a channel plan with extensible encoded args.

Callable only by `channel` itself.


```solidity
function subscribePlan(address channel, uint8 planId, address subscriber, bytes calldata subscriptionArgs)
    external
    payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|
|`planId`|`uint8`|Plan identifier.|
|`subscriber`|`address`|Subscription beneficiary.|
|`subscriptionArgs`|`bytes`|ABI-encoded subscription metadata args (currently supports `(string subscriptionTokenURI)`).|


### unsubscribePlan

Unsubscribes `subscriber` from the channel plan.

Callable only by `channel` itself.


```solidity
function unsubscribePlan(address channel, uint8 planId, address subscriber) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|
|`planId`|`uint8`|Plan identifier.|
|`subscriber`|`address`|Subscription beneficiary.|


### hasActiveSubscription

Returns true when the account has an active subscription or alternative channel access.


```solidity
function hasActiveSubscription(address channel, address account) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|
|`account`|`address`|Account to check.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True when active subscription or alternative access exists.|


## Events
### AccountSubscribed
Emitted when an account subscribes to a plan.


```solidity
event AccountSubscribed(
    address indexed subscriber, address indexed channel, uint8 indexed planId, uint256 expiry, string tokenUri
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriber`|`address`|Subscriber address.|
|`channel`|`address`|Channel address.|
|`planId`|`uint8`|Plan identifier.|
|`expiry`|`uint256`|Subscription expiration timestamp.|
|`tokenUri`|`string`|Metadata token URI emitted related to the subscription token.|

### AccountUnsubscribed
Emitted when an account unsubscribes from a plan.


```solidity
event AccountUnsubscribed(address indexed subscriber, address indexed channel, uint8 indexed planId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriber`|`address`|Subscriber address.|
|`channel`|`address`|Channel address.|
|`planId`|`uint8`|Plan identifier.|

### PlanCreated
`PlanCreated` event is triggerd each time a new subscription plan is added to the contract
We basically need this event to keep on track and process sync of new creted plans


```solidity
event PlanCreated(
    address indexed channel, uint8 indexed planId, address payToken, uint256 price, uint256 duration, string planURI
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`||
|`planId`|`uint8`|Identification of the subscription plan|
|`payToken`|`address`|Token used for the payment|
|`price`|`uint256`|The Price of the target subscription|
|`duration`|`uint256`|Period of validity of the new created plan|
|`planURI`|`string`||

### PlanRemoved
We trigger this event when a given plan is removed from the contract


```solidity
event PlanRemoved(address indexed channel, uint8 indexed planId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`||
|`planId`|`uint8`|Identification of the subscription plan|

### PlanUpdated
we trigger this event when a plan is updated, all information are basically
overwritten at once. Need to make sure there is changes from application side to avoid
unecessary transaction if no data has changed


```solidity
event PlanUpdated(
    address indexed channel, uint8 indexed planId, address payToken, uint256 price, uint256 duration, string planURI
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`||
|`planId`|`uint8`|Identification of the subscription plan|
|`payToken`|`address`|Token used for the payment|
|`price`|`uint256`|The Price of the target subscription|
|`duration`|`uint256`|Period of validity of the new created plan|
|`planURI`|`string`||

## Errors
### PlanNotFound
Error thrown when the plan doesn't exist


```solidity
error PlanNotFound(uint8 planId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`planId`|`uint8`|Identification of the subscription plan|

### InvalidChannelCaller
Thrown when a function is called by an address different from the channel.


```solidity
error InvalidChannelCaller(address expectedChannel, address caller);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`expectedChannel`|`address`|Channel address expected as caller.|
|`caller`|`address`|Actual caller address.|

### UnauthorizedPlanManager
Thrown when the provided plan manager actor is not authorized on the channel.


```solidity
error UnauthorizedPlanManager(address channel, address actor);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|
|`actor`|`address`|Actor attempting to mutate plans.|

### NotSubscribed
Thrown when a subscription-dependent action is requested without an active plan.


```solidity
error NotSubscribed(uint8 planId, address subscriber);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`planId`|`uint8`|Identification of the subscription plan|
|`subscriber`|`address`|Address of the subscriber|

### ActiveSubscriptionNotExpired
Thrown when trying to resubscribe before the active subscription expires.


```solidity
error ActiveSubscriptionNotExpired(address subscriber);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriber`|`address`|Address of the subscriber|

## Structs
### Subscription
Canonical subscription record stored for each subscriber.


```solidity
struct Subscription {
    uint8 planId;
    address subscriber;
    uint256 timestamp;
    uint256 expiry;
    bool recurring;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`planId`|`uint8`|Selected plan identifier.|
|`subscriber`|`address`|Subscriber account.|
|`timestamp`|`uint256`|Subscription start timestamp.|
|`expiry`|`uint256`|Subscription expiration timestamp.|
|`recurring`|`bool`|Whether renewal is expected to recur automatically.|

