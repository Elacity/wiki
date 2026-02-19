## SubscriptionModule

This module is in charge of handling subscription on top of ERC-1155 standard.

We introduce some few constraints to the `ERC-1155` standard for a better management of the flow:

- all plan are considered as fungible token. Note the ownership is not really meaningfull here,
we rather set it as token to be able to keep track of number of subscribers ever to each plan
- a planId is set as `uint8` which allow us to host only 255 plans max. The planId is converted
to a `bytes16` to form a bit-mask to facilitate identification and subscriptions tracking
the plan mask is under `0xffXX0000000000000000000000000000` format, `XX` part determine the `planId`
- in order to be able to register a big amount of subscriptions, each of plan can support all the range
of the 28 least significant bits covered by the plan mask
- when a user subscribe to a plan XX, we mint a new token on `0xffXX0000000000000000000000000000` and
another one as NFT by iterating this mask.
eg. 1st subscriber on `XX` plan will issue a new NFT with `tokenId=0xffXX0000000000000000000000000001`
We make such a calculation as follow:

```solidity
uint128 subscriptionTokenId = planMask + totalSupply(planMask) + 1
```

### NullDurationError

```solidity
error NullDurationError()
```

Duration of the subscription cannot be NULL

### NullPriceError

```solidity
error NullPriceError()
```

Price cannot be NULL

### NotExpiredError

```solidity
error NotExpiredError(address subscriber)
```

The subscriber still have an active subscription

### InsufficientBalanceError

```solidity
error InsufficientBalanceError(address owner, uint256 balance, uint256 amount)
```

The balance if not enough to fulfill the operation

### PLAN_MANAGER

```solidity
bytes32 PLAN_MANAGER
```

### plans

```solidity
mapping(uint8 => struct ISubscriptionManageable.SubscriptionPlan) plans
```

### royaltyHolders

```solidity
struct EnumerableSet.AddressSet royaltyHolders
```

### planExists

```solidity
modifier planExists(uint8 _planId)
```

Modifier that check whether a plan exists

### __SubscriptionModule_init

```solidity
function __SubscriptionModule_init(address _owner) internal
```

### nextPlanId

```solidity
function nextPlanId() external view returns (uint8)
```

Determine the next plan Id for a plan to be created

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint8 | `uint8`-formatted value of the next plan Id |

### _createPlan

```solidity
function _createPlan(address payToken, uint256 price, uint256 duration) internal
```

Create a new subscription plan onto the channel

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| payToken | address | Token used for the payment |
| price | uint256 | Price of the subscription |
| duration | uint256 | Period of the subscription |

### _updatePlan

```solidity
function _updatePlan(uint8 planId, address payToken, uint256 price, uint256 duration) internal
```

Update a subscription plan

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| planId | uint8 | Identification of the subscription plan |
| payToken | address | Token used for the payment |
| price | uint256 | The Price of the target subscription |
| duration | uint256 | Period of validity of the new created plan |

### _removePlan

```solidity
function _removePlan(uint8 planId) internal
```

Remove a subscription plan from the channel. This should not
discontinue any subscriptions that have been registered before
execution of the method

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| planId | uint8 | Identification of the subscription plan |

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

### getPlans

```solidity
function getPlans() external view returns (struct ISubscriptionManageable.SubscriptionPlan[])
```

Retrieve all available subscription plan hooked up to the contract

### _subscribedOn

```solidity
function _subscribedOn(uint8 planId) internal view returns (uint256, bytes16)
```

Count number of subscriptions to a plan regardless of their validity state

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| planId | uint8 | Identification of the subscription plan |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Total number of subscriptions |
| [1] | bytes16 | Mask of the plan |

### subscribePlan

```solidity
function subscribePlan(uint8 planId, bool recurring) external payable
```

Subscribes `msg.sender` to a plan.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| planId | uint8 | Plan identifier. |
| recurring | bool | Whether the subscription should be recurring. |

### unsubscribePlan

```solidity
function unsubscribePlan(uint8 planId) external
```

### hasActiveSubscription

```solidity
function hasActiveSubscription(address account) public view virtual returns (bool)
```

### royaltyInfo

```solidity
function royaltyInfo(uint256 _salePrice) public view returns (struct IERC2981Enhanced.RoyaltyInfo[])
```

### _remapRoyaltyHoldings

```solidity
function _remapRoyaltyHoldings(address from, address to, uint256 amount) internal
```

### hasTradeAccess

```solidity
function hasTradeAccess(address account, uint256 tkId) public view returns (bool)
```

### tokenURI

```solidity
function tokenURI(uint256 tokenId) public view virtual returns (string)
```

Get the calculated value of the `tokenURI` by replacing `{id}` to the `tokenId` value
If the tokenURI is set for that specific `tokenId` it will be returned

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| tokenId | uint256 | Identification of the token |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | string | URI of the json-formatted metadata |

### uri

```solidity
function uri(uint256 tokenId) public view returns (string)
```

### _update

```solidity
function _update(address from, address to, uint256[] ids, uint256[] values) internal virtual
```

### setPaymentProcessor

```solidity
function setPaymentProcessor(address _payProc) external
```

Restrict payment processor changes to admin role

### supportsInterface

```solidity
function supportsInterface(bytes4 interfaceId) public view virtual returns (bool)
```

