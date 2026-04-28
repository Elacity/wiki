# SubscriptionManager
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/modules/subscription/SubscriptionManager.sol)

**Inherits:**
Initializable, OwnableUpgradeable, [ISubscriptionManager](/contracts/modules/subscription/ISubscriptionManager.md)

**Title:**
SubscriptionManager

Singleton manager that stores channel-scoped subscription plans and subscriptions.
Channel contracts keep user-facing APIs and forward calls into this manager.


## State Variables
### PLAN_MANAGER
Plan manager role hash expected on channel contracts.


```solidity
bytes32 public constant PLAN_MANAGER = keccak256("PLAN_MANAGER")
```


### _channelSubscriptions
Channel to isolated subscription storage mapping.


```solidity
mapping(address channel => ChannelSubscriptionStorage data) private _channelSubscriptions
```


### _subscribePlanLock
Channel to reentrancy lock mapping for subscribe flows.


```solidity
mapping(address channel => uint256 lock) private _subscribePlanLock
```


## Functions
### constructor

**Notes:**
- oz-upgrades-unsafe-allow: constructor

- docs-ignore: true


```solidity
constructor() ;
```

### initialize

Initializes manager ownership.


```solidity
function initialize(address owner_) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner_`|`address`|Owner authorized to upgrade and administer the manager.|


### onlyChannel

Restricts entrypoints to the provided channel caller.


```solidity
modifier onlyChannel(address channel) ;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Expected channel caller.|


### planExists

Ensures the target channel plan exists and is active.


```solidity
modifier planExists(address channel, uint8 planId) ;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|
|`planId`|`uint8`|Plan identifier.|


### subscribePlanNonReentrant

Prevents nested subscribe calls per channel.


```solidity
modifier subscribePlanNonReentrant(address channel) ;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|


### _onlyChannel

Validates the entrypoint caller is the expected channel.


```solidity
function _onlyChannel(address channel) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Expected channel caller.|


### _planExists

Validates the target plan is active for the channel.


```solidity
function _planExists(address channel, uint8 planId) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|
|`planId`|`uint8`|Plan identifier.|


### _subscribePlanNonReentrantBefore

Acquires per-channel subscribe reentrancy guard.


```solidity
function _subscribePlanNonReentrantBefore(address channel) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|


### _subscribePlanNonReentrantAfter

Releases per-channel subscribe reentrancy guard.


```solidity
function _subscribePlanNonReentrantAfter(address channel) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|


### configureChannel


```solidity
function configureChannel(address channel, ISubscriptionManageable.SubscriptionPlan[] calldata plans)
    external
    onlyChannel(channel);
```

### getPlan

Returns the plan by id for a channel.


```solidity
function getPlan(address channel, uint8 planId)
    external
    view
    onlyChannel(channel)
    returns (ISubscriptionManageable.SubscriptionPlan memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|
|`planId`|`uint8`|Plan identifier.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`ISubscriptionManageable.SubscriptionPlan`|Plan descriptor.|


### nextPlanId

Returns the next plan id for a channel.


```solidity
function nextPlanId(address channel) external view onlyChannel(channel) returns (uint8);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|Next monotonic plan id.|


### getPlans

Returns all plans for a channel.


```solidity
function getPlans(address channel)
    external
    view
    onlyChannel(channel)
    returns (ISubscriptionManageable.SubscriptionPlan[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`ISubscriptionManageable.SubscriptionPlan[]`|plans Full plan array from `1..lastPlanId`.|


### bulkUpdatePlans


```solidity
function bulkUpdatePlans(address channel, ISubscriptionManageable.UpdateAction[] calldata actions, address actor)
    external
    onlyChannel(channel);
```

### subscribePlan

Subscribes `subscriber` to a channel plan with extensible encoded args.

Callable only by `channel` itself.


```solidity
function subscribePlan(address channel, uint8 planId, address subscriber, bytes calldata subscriptionArgs)
    external
    payable
    onlyChannel(channel)
    planExists(channel, planId)
    subscribePlanNonReentrant(channel);
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
function unsubscribePlan(address channel, uint8 planId, address subscriber)
    external
    onlyChannel(channel)
    planExists(channel, planId);
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
function hasActiveSubscription(address channel, address account) external view onlyChannel(channel) returns (bool);
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


### _hasActiveSubscription

Checks active access through alternative access or subscription expiry.


```solidity
function _hasActiveSubscription(address channel, address account) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|
|`account`|`address`|Account to evaluate.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True when account is considered subscribed.|


### _subscribedOn

Computes current subscriber count for a plan via channel token supply.


```solidity
function _subscribedOn(address channel, uint8 planId) internal view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|
|`planId`|`uint8`|Plan identifier.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Current subscriber count.|


### _decodeSubscriptionArgs

Decodes extensible subscription args into channel-side token URI metadata.

Current format is abi.encode(string subscriptionTokenURI). Empty args are allowed.


```solidity
function _decodeSubscriptionArgs(bytes calldata subscriptionArgs)
    internal
    pure
    returns (string memory subscriptionTokenURI);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriptionArgs`|`bytes`|ABI-encoded args payload.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`subscriptionTokenURI`|`string`|Decoded metadata URI, or empty string when omitted.|


### _subscribePlan

Shared subscription workflow used by both default and URI-aware entrypoints.


```solidity
function _subscribePlan(address channel, uint8 planId, address subscriber, string memory subscriptionTokenURI)
    internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|
|`planId`|`uint8`|Plan identifier.|
|`subscriber`|`address`|Subscription beneficiary.|
|`subscriptionTokenURI`|`string`|Optional metadata URI for the newly minted per-subscription token.|


### _authorizePlanManager

Validates actor authorization against channel `PLAN_MANAGER` role.


```solidity
function _authorizePlanManager(address channel, address actor) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|
|`actor`|`address`|Account requesting plan mutation.|


### _createPlan

Creates a new plan in the channel namespace.


```solidity
function _createPlan(address channel, address payToken, uint256 price, uint256 duration, string memory planURI)
    internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|
|`payToken`|`address`|Payment token (zero-address for native).|
|`price`|`uint256`|Plan price.|
|`duration`|`uint256`|Plan duration in seconds.|
|`planURI`|`string`||


### _createPlanFromActionArgs


```solidity
function _createPlanFromActionArgs(address channel, bytes calldata args) internal;
```

### _updatePlan

Updates an existing plan.


```solidity
function _updatePlan(
    address channel,
    uint8 planId,
    address payToken,
    uint256 price,
    uint256 duration,
    string memory planURI
) internal planExists(channel, planId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|
|`planId`|`uint8`|Plan identifier.|
|`payToken`|`address`|Payment token.|
|`price`|`uint256`|Price.|
|`duration`|`uint256`|Duration.|
|`planURI`|`string`||


### _updatePlanFromActionArgs


```solidity
function _updatePlanFromActionArgs(address channel, bytes calldata args) internal;
```

### _removePlan

Deactivates a plan in the channel namespace.


```solidity
function _removePlan(address channel, uint8 planId) internal planExists(channel, planId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Target channel.|
|`planId`|`uint8`|Plan identifier.|


## Errors
### NullDurationError
Duration of the subscription cannot be zero.


```solidity
error NullDurationError();
```

### NullPriceError
Price cannot be zero.


```solidity
error NullPriceError();
```

### InsufficientPayment
Payment is lower than the plan price.


```solidity
error InsufficientPayment(uint256 required, uint256 sent);
```

### PaymentNotAccepted
Native payment is not accepted for ERC-20 plans.


```solidity
error PaymentNotAccepted(uint256 sent);
```

### ReentrantSubscribePlanCall
Raised when `subscribePlan` is called reentrantly.


```solidity
error ReentrantSubscribePlanCall();
```

### NotExpiredError
The subscriber still has an active subscription.


```solidity
error NotExpiredError(address subscriber);
```

### MalformedSubscriptionArgs
Raised when subscription args cannot be decoded.


```solidity
error MalformedSubscriptionArgs();
```

## Structs
### ChannelSubscriptionStorage
Per-channel subscription state container.


```solidity
struct ChannelSubscriptionStorage {
    /**
     * @notice Last created plan id for the channel.
     */
    uint8 lastPlanId;
    /**
     * @notice Plan store keyed by plan id.
     */
    mapping(uint8 => ISubscriptionManageable.SubscriptionPlan) plans;
    /**
     * @notice Subscriber store keyed by subscriber address.
     */
    mapping(address => Subscription) subscriptions;
}
```

