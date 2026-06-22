# ISubscriptionManageable
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/modules/subscription/ISubscriptionManageable.sol)

**Inherits:**
[ISubscriptionPrimitive](/contracts/modules/subscription/ISubscriptionPrimitive.md)

**Title:**
ISubscriptionManageable

Provides methods and structs to manage subscription flow.


## Functions
### nextPlanId

Determine the next plan Id for a plan to be created


```solidity
function nextPlanId() external view returns (uint8);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint8`|`uint8`-formatted value of the next plan Id|


### getPlans

Retrieve all available subscription plan hooked up to the contract


```solidity
function getPlans() external view returns (SubscriptionPlan[] memory);
```

### bulkUpdatePlans

Process a bulk operation of a mix subscription plans
update, it should be an array of update actions which could be
either a plan creation, update or a removal.
Following shoudl be how data is formatted according to action type:
- ADD (1): (address, uint256, uint, string)
- UPDATE (2): (uint8, address, uint256, uint, string)
- REMOVE: (uint8)
an action should then follow (uint8 actionType, bytes memory args)
```typescript
const payload = [
{
// for plan creation
actionType: 1,
args: ethers.utils.defaultAbiCoder.encode(["address", "uint256", "uint", "string"], ["0x0000000000000000000000000000000000000000", BigNumber.from("1000000"), 3600 * 24 * 7, "ipfs://.../plan-1.json"])},
{
// for plan update
actionType: 2,
args: ethers.utils.defaultAbiCoder.encode(["uint8", "address", "uint256", "uint", "string"], [1, "0x0000000000000000000000000000000000000000", BigNumber.from("1000000"), 3600 * 24 * 30, "ipfs://.../plan-1-updated.json"])
},
{
// for plan removal
actionType: 3,
args: ethers.utils.defaultAbiCoder.encode(["uint8"], [2])
}
].map(action => Object,values(action))
```


```solidity
function bulkUpdatePlans(UpdateAction[] calldata actions) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`actions`|`UpdateAction[]`|Array of action for the mutation|


