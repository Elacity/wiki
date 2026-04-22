# ISubscriptionPrimitive
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/modules/subscription/ISubscriptionPrimitive.sol)

**Title:**
ISubscriptionPrimitive

Provides structs for subscription flow.


## Errors
### MalformedArgs
Thrown when an invalid argument is provided


```solidity
error MalformedArgs();
```

## Structs
### SubscriptionPlan
Minimal information needed to define a subscription plan


```solidity
struct SubscriptionPlan {
    /// @notice the ID of the plan
    uint8 planId;
    /// @notice the payment token to be used, supports ERC-20 and native token
    address payToken;
    /// @notice the price in wei of the plan
    uint256 price;
    /// @notice unit of time of the subscription duration
    /// it's in compliance with solidity time unit
    /// see https://docs.soliditylang.org/en/develop/units-and-global-variables.html#time-units
    uint256 duration;
    /// @notice flag of plan removal, set as true for active plans
    bool active;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`planId`|`uint8`|Identification of the subscription plan|
|`payToken`|`address`|Payment token used for the subscription payment, could be ERC-20 or native token|
|`price`|`uint256`|Price to pay for the subscription plan|
|`duration`|`uint256`|Period of validity of the subscription|
|`active`|`bool`|Flag that determine whether a plan have been removed|

### UpdateAction
This sctructure is basically forming the argument for the bulk update operation


```solidity
struct UpdateAction {
    uint8 actionType;
    bytes args;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`actionType`|`uint8`|The action to do: 1 = ADD, 2 = UPDATE, 3 = REMOVE|
|`args`|`bytes`|The raw arguments to parse for the appropriate action|

