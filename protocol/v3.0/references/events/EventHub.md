# EventHub
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/events/EventHub.sol)

**Inherits:**
Initializable, OwnableUpgradeable, [IEventHub](/contracts/events/IEventHub.md)

**Title:**
EventHub

Stateless, centralized emitter contract for watcher-facing protocol events.

Only acknowledged protocol contracts are allowed to emit.


## State Variables
### systemTracker
Central system tracker used for emitter authorization checks.


```solidity
ISystemTracker public systemTracker
```


## Functions
### onlyAcknowledgedEmitter

Restricts event emission to acknowledged protocol contracts.


```solidity
modifier onlyAcknowledgedEmitter() ;
```

### constructor

**Notes:**
- oz-upgrades-unsafe-allow: constructor

- docs-ignore: true


```solidity
constructor() ;
```

### initialize

Initializes the EventHub.


```solidity
function initialize(address initialOwner, address systemTrackerAddress) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`initialOwner`|`address`|Owner account for administrative updates.|
|`systemTrackerAddress`|`address`|Address of the protocol system tracker.|


### setSystemTracker

Updates the system tracker reference.


```solidity
function setSystemTracker(address systemTrackerAddress) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`systemTrackerAddress`|`address`|Address of the new system tracker.|


### emitAssetCreated

Emits an {AssetCreated} event through the hub.


```solidity
function emitAssetCreated(
    address _to,
    address _channel,
    uint256 _tokenId,
    string calldata _tokenUri,
    uint16 _opType,
    address opContract
) external onlyAcknowledgedEmitter;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_to`|`address`|Beneficiary that receives the asset.|
|`_channel`|`address`|Channel contract address where the asset is hosted.|
|`_tokenId`|`uint256`|Asset token identifier.|
|`_tokenUri`|`string`|Metadata URI of the asset.|
|`_opType`|`uint16`|Operative type used for the asset flow.|
|`opContract`|`address`|Operative contract address (zero when none).|


### emitPaymentLog

Emits a {PaymentLog} event through the hub.


```solidity
function emitPaymentLog(address context, address from, address to, uint256 amount, address paymentToken)
    external
    onlyAcknowledgedEmitter;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`context`|`address`|Contract from where the payment have been initiated (e.g. Channel/PaymentModule-inherited).|
|`from`|`address`|Payment source.|
|`to`|`address`|Payment recipient.|
|`amount`|`uint256`|Transfer amount.|
|`paymentToken`|`address`|Payment token (zero for native).|


### emitPaymentCommitted

Emits a {PaymentCommitted} event through the hub.


```solidity
function emitPaymentCommitted(
    address context,
    address from,
    address recipient,
    address paymentToken,
    uint256 amount
) external onlyAcknowledgedEmitter;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`context`|`address`|Contract from where the payment have been initiated (e.g. WithdrawablePaymentProcessor).|
|`from`|`address`|Payer address.|
|`recipient`|`address`|Recipient contract/account.|
|`paymentToken`|`address`|Payment token (zero for native).|
|`amount`|`uint256`|Committed amount.|


### emitRewardsWithdrawn

Emits a {RewardsWithdrawn} event through the hub.


```solidity
function emitRewardsWithdrawn(address context, address to, uint256 amount, address paymentToken)
    external
    onlyAcknowledgedEmitter;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`context`|`address`|Contract from where the payment have been initiated (e.g. RewardsRecipient-inherited).|
|`to`|`address`|Beneficiary.|
|`amount`|`uint256`|Withdrawn amount.|
|`paymentToken`|`address`|Token of withdrawn rewards.|


### emitRewardsIncremented

Emits a {RewardsIncremented} event through the hub.


```solidity
function emitRewardsIncremented(
    address context,
    address to,
    uint256 amount,
    address paymentToken,
    address _processor
) external onlyAcknowledgedEmitter;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`context`|`address`|Contract from where the payment have been initiated (e.g. RewardsRecipient-inherited).|
|`to`|`address`|Beneficiary.|
|`amount`|`uint256`|Credited amount.|
|`paymentToken`|`address`|Token of credited rewards.|
|`_processor`|`address`|Payment processor that issued credit.|


### emitAtomicNativeTransfer

Emits an {AtomicNativeTransfer} event through the hub.


```solidity
function emitAtomicNativeTransfer(address from, address to, uint256 amount) external onlyAcknowledgedEmitter;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Caller that initiated transfer.|
|`to`|`address`|Recipient address for custody transfer.|
|`amount`|`uint256`|Native amount transferred.|


### emitTokenAccessRegistered

Emits a {TokenAccessRegistered} event through the hub.


```solidity
function emitTokenAccessRegistered(address context, address _tokenAddress, uint256 threshold)
    external
    onlyAcknowledgedEmitter;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`context`|`address`||
|`_tokenAddress`|`address`|Token address used for access checks.|
|`threshold`|`uint256`|Minimum token balance threshold.|


### emitTokenAccessRemoved

Emits a {TokenAccessRemoved} event through the hub.


```solidity
function emitTokenAccessRemoved(address context, address _tokenAddress) external onlyAcknowledgedEmitter;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`context`|`address`||
|`_tokenAddress`|`address`|Token address removed from access checks.|


### _onlyAcknowledgedEmitter

Ensures caller is acknowledged in the system tracker.


```solidity
function _onlyAcknowledgedEmitter() internal view;
```

### _setSystemTracker


```solidity
function _setSystemTracker(address systemTrackerAddress) internal;
```

## Events
### SystemTrackerUpdated
Emitted when the system tracker reference is updated.


```solidity
event SystemTrackerUpdated(address indexed previousTracker, address indexed newTracker);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`previousTracker`|`address`|Previous tracker address.|
|`newTracker`|`address`|New tracker address.|

## Errors
### UnauthorizedEmitter
Thrown when a non-acknowledged caller attempts to emit.


```solidity
error UnauthorizedEmitter(address emitter);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`emitter`|`address`|Unauthorized caller.|

### InvalidSystemTracker
Thrown when attempting to set a zero tracker address.


```solidity
error InvalidSystemTracker(address systemTrackerAddress);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`systemTrackerAddress`|`address`|Invalid tracker address.|

