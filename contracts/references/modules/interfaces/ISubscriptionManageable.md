## ISubscriptionManageable

Provides methods and structs to manage subscription flow.

### PlanNotFound

```solidity
error PlanNotFound(uint8 planId)
```

Error thrown when the plan doesn't exist

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| planId | uint8 | Identification of the subscription plan |

### MalformedArgs

```solidity
error MalformedArgs()
```

### SubscriptionPlan

Minimal information needed to define a subscription plan

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

```solidity
struct SubscriptionPlan {
  uint8 planId;
  address payToken;
  uint256 price;
  uint256 duration;
  bool active;
}
```

### UpdateAction

This sctructure is basically forming the argument for the bulk update operation

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

```solidity
struct UpdateAction {
  string actionType;
  bytes args;
}
```

### PlanCreated

```solidity
event PlanCreated(uint8 planId, address payToken, uint256 price, uint256 duration)
```

`PlanCreated` event is triggerd each time a new subscription plan is added to the contract
We basically need this event to keep on track and process sync of new creted plans

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| planId | uint8 | Identification of the subscription plan |
| payToken | address | Token used for the payment |
| price | uint256 | The Price of the target subscription |
| duration | uint256 | Period of validity of the new created plan |

### PlanRemoved

```solidity
event PlanRemoved(uint8 planId)
```

We trigger this event when a given plan is removed from the contract

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| planId | uint8 | Identification of the subscription plan |

### PlanUpdated

```solidity
event PlanUpdated(uint8 planId, address payToken, uint256 price, uint256 duration)
```

@notice we trigger this event when a plan is updated, all information are basically
overwritten at once. Need to make sure there is changes from application side to avoid
unecessary transaction if no data has changed

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| planId | uint8 | Identification of the subscription plan |
| payToken | address | Token used for the payment |
| price | uint256 | The Price of the target subscription |
| duration | uint256 | Period of validity of the new created plan |

### nextPlanId

```solidity
function nextPlanId() external view returns (uint8)
```

Determine the next plan Id for a plan to be created

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint8 | `uint8`-formatted value of the next plan Id |

### getPlans

```solidity
function getPlans() external view returns (struct ISubscriptionManageable.SubscriptionPlan[])
```

Retrieve all available subscription plan hooked up to the contract

### bulkUpdatePlans

```solidity
function bulkUpdatePlans(struct ISubscriptionManageable.UpdateAction[] actions) external
```

Process a bulk operation of a mix subscription plans
update, it should be an array of update actions which could be
either a plan creation, update or a removal.

Following shoudl be how data is formatted according to action type:
- ADD: (address, uint256, uint)
- UPDATE: (uint8, address, uint256, uint)
- REMOVE: (uint8)

an action should then follow (string actionType, bytes memory args)

```typescript
const payload = [
  {
    // for plan creation
    actionType: 'ADD',
    args: ethers.utils.defaultAbiCoder.encode(["address", "uint256", "uint"], ["0x0000000000000000000000000000000000000000", BigNumber.from("1000000"), 3600 * 24 * 7])},
  {
    // for plan update
    actionType: 'UPDATE',
    args: ethers.utils.defaultAbiCoder.encode(["uint8", "address", "uint256", "uint"], [1, "0x0000000000000000000000000000000000000000", BigNumber.from("1000000"), 3600 * 24 * 30])
  },
  {
    // for plan removal
    actionType: 'REMOVE',
    args: ethers.utils.defaultAbiCoder.encode(["uint8"], [2])
  }
].map(action => Object,values(action))
```

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| actions | struct ISubscriptionManageable.UpdateAction[] | Array of action for the mutation |

