# RoyaltyModule
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/modules/royalty/RoyaltyModule.sol)

**Title:**
RoyaltyModule

Shared royalty constants, structs, and validation errors.


## State Variables
### MAX_ROYALTY_SUPPLY
Maximum total supply of ROYALTY_SHARE tokens per contract.


```solidity
uint256 public constant MAX_ROYALTY_SUPPLY = 1000
```


## Errors
### RoyaltyCapExceeded
Thrown when a royalty mint would exceed the maximum supply cap.


```solidity
error RoyaltyCapExceeded(uint256 requested, uint256 max);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`requested`|`uint256`|Total supply that would result from the mint.|
|`max`|`uint256`|The maximum allowed supply.|

## Structs
### ShareInput
Input that defines a royalty distribution.


```solidity
struct ShareInput {
    /**
     * @notice Address of the beneficiary.
     */
    address beneficiary;
    /**
     * @notice Share units out of MAX_ROYALTY_SUPPLY.
     */
    uint256 share;
}
```

