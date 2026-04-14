# SystemTracker
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/storage/SystemTracker.sol)

**Inherits:**
[ISystemTracker](/contracts/storage/ISystemTracker.md), OwnableUpgradeable

**Title:**
SystemTracker

Tracks protocol-recognized contracts and stores deterministic slot-based contract addresses.

Mounted on the root storage contract to provide:
1) acknowledgement/whitelisting for trusted protocol contracts,
2) contract address lookup by slot key.


## State Variables
### SYSTEM_TRACKER_STORAGE_LOCATION
Storage slot for SystemTrackerStorage.
Formula: keccak256(abi.encode(uint256(keccak256("elacity.drm.storage.SystemTracker")) - 1)) & ~bytes32(uint256(0xff))


```solidity
bytes32 private constant SYSTEM_TRACKER_STORAGE_LOCATION =
    0x5ac45e62345a08ca075c80adb0adc9c357e0220b8fed73d7ea4198bfc2bf1c00
```


## Functions
### _getSystemTrackerStorage

Retrieves ERC-7201 namespaced storage.


```solidity
function _getSystemTrackerStorage() private pure returns (SystemTrackerStorage storage $);
```

### ack

Acknowledges a contract, marking it as part of the ecosystem.

Only owner or explicit ack authorities can acknowledge contracts.


```solidity
function ack(address ca) external virtual;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ca`|`address`|Contract address to acknowledge.|


### unAck

Removes a contract from the ecosystem.

Only owner can remove contracts from ecosystem.


```solidity
function unAck(address ca) external virtual onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ca`|`address`|Contract address to remove.|


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

Reverts when `roleMask == 0` to prevent ambiguous no-op role updates.
Enabling role bits automatically acknowledges the contract for global legacy trust checks.


```solidity
function setContractRoles(address contractAddress, uint256 roleMask, bool enabled) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`contractAddress`|`address`|Contract to mutate role bits for.|
|`roleMask`|`uint256`|       Role bitmask to toggle.|
|`enabled`|`bool`|        `true` to enable bits, `false` to disable bits.|


### hasContractRoles

Returns whether a contract has all role bits required by `roleMask`.

Returns `false` when `roleMask == 0`.


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


### setAckAuthority

Enables or disables a trusted ack authority.

Only owner can manage ack authorities.


```solidity
function setAckAuthority(address authority, bool enabled) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`authority`|`address`|Address of the ack authority.|
|`enabled`|`bool`|  Whether the ack authority is enabled.|


### isAckAuthority

Returns whether an address is allowed to acknowledge contracts.


```solidity
function isAckAuthority(address authority) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`authority`|`address`|Address of the ack authority.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|True if the address is an ack authority, false otherwise.|


### _registerAt

Registers a contract address at a slot.


```solidity
function _registerAt(bytes32 slot, address value) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`slot`|`bytes32`|Slot key used to store the contract address.|
|`value`|`address`|Contract address to register.|


### registerAt

**Note:**
docs-ignore: true


```solidity
function registerAt(bytes32 slot, address value, uint256 roleMask) external onlyOwner;
```

### registerAt

**Note:**
docs-ignore: true


```solidity
function registerAt(string memory slotStr, address value, uint256 roleMask) external onlyOwner;
```

### _setContractRoles


```solidity
function _setContractRoles(address contractAddress, uint256 roleMask, bool enabled) internal;
```

## Structs
### SystemTrackerStorage
**Note:**
storage-location: erc7201:elacity.drm.storage.SystemTracker


```solidity
struct SystemTrackerStorage {
    /// @notice Additional trusted contracts that can acknowledge newly-created protocol contracts.
    /// @dev Either an authority contract or the Owner can acknowledge contracts.
    mapping(address => bool) ackAuthorities;
    /// @notice Determine whether a contract is apart of the ecosystem.
    mapping(address => bool) acknowledged;
    /// @notice Mapping from slot identifier to contract address.
    mapping(bytes32 => address) contractAt;
    /// @notice Capability role bitmask per contract.
    mapping(address => uint256) contractRoles;
}
```

