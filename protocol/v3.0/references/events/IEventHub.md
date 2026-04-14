# IEventHub
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/events/IEventHub.sol)

**Title:**
IEventHub

Centralized event surface for watcher-relevant protocol events.

Contracts emit by calling dedicated `emit*` functions on the hub.


## Functions
### emitAssetCreated

Emits an [AssetCreated](/contracts/events/IEventHub.md#assetcreated) event through the hub.


```solidity
function emitAssetCreated(
    address _to,
    address _channel,
    uint256 _tokenId,
    string calldata _tokenUri,
    uint16 _opType,
    address opContract
) external;
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


### emitTokenAccessRegistered

Emits a [TokenAccessRegistered](/contracts/events/IEventHub.md#tokenaccessregistered) event through the hub.


```solidity
function emitTokenAccessRegistered(address context, address _tokenAddress, uint256 threshold) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`context`|`address`||
|`_tokenAddress`|`address`|Token address used for access checks.|
|`threshold`|`uint256`|Minimum token balance threshold.|


### emitTokenAccessRemoved

Emits a [TokenAccessRemoved](/contracts/events/IEventHub.md#tokenaccessremoved) event through the hub.


```solidity
function emitTokenAccessRemoved(address context, address _tokenAddress) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`context`|`address`||
|`_tokenAddress`|`address`|Token address removed from access checks.|


### emitPaymentLog

Emits a [PaymentLog](/contracts/events/IEventHub.md#paymentlog) event through the hub.


```solidity
function emitPaymentLog(address context, address from, address to, uint256 amount, address paymentToken) external;
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

Emits a [PaymentCommitted](/contracts/events/IEventHub.md#paymentcommitted) event through the hub.


```solidity
function emitPaymentCommitted(
    address context,
    address from,
    address recipient,
    address paymentToken,
    uint256 amount
) external;
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

Emits a [RewardsWithdrawn](/contracts/events/IEventHub.md#rewardswithdrawn) event through the hub.


```solidity
function emitRewardsWithdrawn(address context, address to, uint256 amount, address paymentToken) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`context`|`address`|Contract from where the payment have been initiated (e.g. RewardsRecipient-inherited).|
|`to`|`address`|Beneficiary.|
|`amount`|`uint256`|Withdrawn amount.|
|`paymentToken`|`address`|Token of withdrawn rewards.|


### emitRewardsIncremented

Emits a [RewardsIncremented](/contracts/events/IEventHub.md#rewardsincremented) event through the hub.


```solidity
function emitRewardsIncremented(
    address context,
    address to,
    uint256 amount,
    address paymentToken,
    address _processor
) external;
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

Emits an [AtomicNativeTransfer](/contracts/events/IEventHub.md#atomicnativetransfer) event through the hub.


```solidity
function emitAtomicNativeTransfer(address from, address to, uint256 amount) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Caller that initiated transfer.|
|`to`|`address`|Recipient address for custody transfer.|
|`amount`|`uint256`|Native amount transferred.|


## Events
### AssetCreated
Emitted when a digital asset is minted.


```solidity
event AssetCreated(
    address indexed _to,
    address indexed _channel,
    uint256 _tokenId,
    string _tokenUri,
    uint16 _opType,
    address indexed opContract
);
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

### DigitalAssetRegistered
Emitted when a digital asset is fully registered with content metadata.


```solidity
event DigitalAssetRegistered(
    address indexed channel, uint256 indexed tokenId, address indexed operative, uint16 opType, bytes16 contentId
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Channel contract address that owns the asset.|
|`tokenId`|`uint256`|Asset token identifier.|
|`operative`|`address`|Operative contract address.|
|`opType`|`uint16`|Operative type used for the asset flow.|
|`contentId`|`bytes16`|Content identifier bound to the asset.|

### PaymentLog
Emitted when a payment transfer is processed.


```solidity
event PaymentLog(
    address indexed context, address indexed from, address indexed to, uint256 amount, address paymentToken
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`context`|`address`|Contract from where the payment have been initiated.|
|`from`|`address`|Payment source.|
|`to`|`address`|Payment recipient.|
|`amount`|`uint256`|Transfer amount.|
|`paymentToken`|`address`|Payment token (zero for native).|

### PaymentCommitted
Emitted when deferred payment batches are committed.


```solidity
event PaymentCommitted(
    address indexed context, address indexed from, address indexed recipient, address paymentToken, uint256 amount
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`context`|`address`|Contract from where the payment have been initiated (e.g. WithdrawablePaymentProcessor).|
|`from`|`address`|Payer address.|
|`recipient`|`address`|Recipient contract/account.|
|`paymentToken`|`address`|Payment token (zero for native).|
|`amount`|`uint256`|Committed amount.|

### RewardsWithdrawn
Emitted when rewards are withdrawn.


```solidity
event RewardsWithdrawn(address indexed context, address indexed to, uint256 amount, address indexed paymentToken);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`context`|`address`|Contract from where the payment have been initiated (e.g. RewardsRecipient-inherited).|
|`to`|`address`|Beneficiary.|
|`amount`|`uint256`|Withdrawn amount.|
|`paymentToken`|`address`|Token of withdrawn rewards.|

### RewardsIncremented
Emitted when rewards are credited.


```solidity
event RewardsIncremented(
    address indexed context, address indexed to, uint256 amount, address indexed paymentToken, address _processor
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`context`|`address`|Contract from where the payment have been initiated (e.g. RewardsRecipient-inherited).|
|`to`|`address`|Beneficiary.|
|`amount`|`uint256`|Credited amount.|
|`paymentToken`|`address`|Token of credited rewards.|
|`_processor`|`address`|Payment processor that issued credit.|

### AtomicNativeTransfer
Emitted when native payment is atomically transferred into processor custody.


```solidity
event AtomicNativeTransfer(address indexed from, address indexed to, uint256 amount);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Caller that initiated transfer.|
|`to`|`address`|Recipient address for custody transfer.|
|`amount`|`uint256`|Native amount transferred.|

### TokenAccessRegistered
Emitted when token-ownership access is registered.


```solidity
event TokenAccessRegistered(address indexed context, address indexed _tokenAddress, uint256 threshold);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`context`|`address`|Contract from where the token-ownership access have been initiated (e.g. TokenOwnershipModule).|
|`_tokenAddress`|`address`|Token address used for access checks.|
|`threshold`|`uint256`|Minimum token balance threshold.|

### TokenAccessRemoved
Emitted when token-ownership access is removed.


```solidity
event TokenAccessRemoved(address indexed context, address indexed _tokenAddress);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`context`|`address`|Contract from where the token-ownership access have been initiated (e.g. TokenOwnershipModule).|
|`_tokenAddress`|`address`|Token address removed from access checks.|

