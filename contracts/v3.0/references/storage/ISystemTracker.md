# ISystemTracker
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/storage/ISystemTracker.sol)

**Title:**
ISystemTracker

Interface for protocol system tracking and slot-based contract registry access.


## Functions
### ack

Recognizes/whitelists a contract as part of the protocol system.

**Note:**
docs-ignore: true


```solidity
function ack(address contractAddress) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`contractAddress`|`address`|Contract address to add.|


### unAck

Removes a contract from the recognized system set.

**Note:**
docs-ignore: true


```solidity
function unAck(address contractAddress) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`contractAddress`|`address`|Contract address to remove.|


### acknowledged

Returns whether a contract is recognized as a system contract.

**Note:**
docs-ignore: true


```solidity
function acknowledged(address contractAddress) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`contractAddress`|`address`|Contract address to check.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|`true` if recognized.|


### setContractRoles

Assigns or revokes role bits for a contract.

Owner-only on the storage implementation.
Enabling any role bits also marks `contractAddress` as acknowledged globally.


```solidity
function setContractRoles(address contractAddress, uint256 roleMask, bool enabled) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`contractAddress`|`address`|Contract to mutate role bits for.|
|`roleMask`|`uint256`|       Role bitmask to toggle.|
|`enabled`|`bool`|        `true` to enable bits, `false` to disable bits.|


### hasContractRoles

Returns whether a contract has all role bits required by `roleMask`.


```solidity
function hasContractRoles(address contractAddress, uint256 roleMask) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`contractAddress`|`address`|Contract address to query.|
|`roleMask`|`uint256`|       Required role bitmap.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|`true` if all bits in `roleMask` are set for `contractAddress`.|


### contractAt

Returns the contract address registered at a slot.


```solidity
function contractAt(bytes32 slot) external view returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`slot`|`bytes32`|Storage slot key to query.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Contract address stored at `slot`.|


### registerAt

Registers a contract address at a slot.

**Note:**
docs-ignore: true


```solidity
function registerAt(bytes32 slot, address value, uint256 roleMask) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`slot`|`bytes32`|Storage slot key to write.|
|`value`|`address`|Contract address to register.|
|`roleMask`|`uint256`|Role bitmask to assign to `value`. Set to `0` to register without assigning any role.|


### registerAt

Registers a contract address at a named slot.

**Note:**
docs-ignore: true


```solidity
function registerAt(string memory slotStr, address value, uint256 roleMask) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`slotStr`|`string`|Slot name to hash and write.|
|`value`|`address`|Contract address to register.|
|`roleMask`|`uint256`|Role bitmask to assign to `value`. Set to `0` to register without assigning any role.|


## Events
### ContractAcknowledged
Emitted when a contract is acknowledged by the protocol tracker.


```solidity
event ContractAcknowledged(address indexed contractAddress);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`contractAddress`|`address`|Contract address that was acknowledged.|

### ContractUnacknowledged
Emitted when a contract is removed from the protocol acknowledged set.


```solidity
event ContractUnacknowledged(address indexed contractAddress);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`contractAddress`|`address`|Contract address that was unacknowledged.|

### AckAuthorityUpdated
Emitted when an ack authority is updated.


```solidity
event AckAuthorityUpdated(address indexed authority, bool enabled);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`authority`|`address`|Address of the ack authority.|
|`enabled`|`bool`|  Whether the ack authority is enabled.|

### ContractRolesUpdated
Emitted when one or more contract roles are updated.


```solidity
event ContractRolesUpdated(address indexed contractAddress, uint256 indexed roleMask, bool enabled);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`contractAddress`|`address`|Contract address whose roles changed.|
|`roleMask`|`uint256`|       Bitmask of roles toggled.|
|`enabled`|`bool`|        Whether the bits in `roleMask` were enabled or disabled.|

## Errors
### UnauthorizedAckError
Thrown when a caller without the necessary permissions attempts to acknowledge a contract.


```solidity
error UnauthorizedAckError(address caller);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`caller`|`address`|The address that attempted to acknowledge the contract.|

### InvalidRoleMask
Thrown when trying to assign roles with an empty bitmask.


```solidity
error InvalidRoleMask(uint256 roleMask);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`roleMask`|`uint256`|Empty role mask provided by caller.|

