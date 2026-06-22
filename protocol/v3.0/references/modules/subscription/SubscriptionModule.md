# SubscriptionModule
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/modules/subscription/SubscriptionModule.sol)

**Inherits:**
[ISubscribable](/contracts/modules/subscription/ISubscribable.md), [ISubscriptionManageable](/contracts/modules/subscription/ISubscriptionManageable.md), AccessControlUpgradeable, IERC1155, ERC1155Upgradeable, ERC1155SupplyUpgradeable, ERC1155URIStorageUpgradeable, [RoyaltyPayoutModule](/contracts/modules/royalty/RoyaltyPayoutModule.md), [IERC2981Enhanced](/contracts/modules/royalty/IERC2981Enhanced.md), [AccessControlExclusiveTransferrableTokens](/contracts/modules/library/AccessControlExclusiveTransferrableTokens.md), [TokenOwnershipModule](/contracts/modules/access-control/TokenOwnershipModule.md), [TradeAccessRestriction](/contracts/modules/trade/TradeAccessRestriction.md), [RewardsRecipient](/contracts/modules/payment/RewardsRecipient.md), [RoyaltyModule](/contracts/modules/royalty/RoyaltyModule.md)

**Title:**
SubscriptionModule

Channel-side module for ERC-1155 minting, royalty distribution, and trade access.
Subscription state is delegated to a singleton `SubscriptionManager`.


## State Variables
### PLAN_MANAGER
Role identifier for accounts allowed to create, update, and remove subscription plans.


```solidity
bytes32 public constant PLAN_MANAGER = keccak256("PLAN_MANAGER")
```


### SUBSCRIPTION_TOKEN_STORAGE_LOCATION

```solidity
bytes32 private constant SUBSCRIPTION_TOKEN_STORAGE_LOCATION =
    0x12a9fadcf2ac7977af21622d175f9587be039ed3d0ae0f87dae30425bc13a000
```


### subscriptionManager
Subscription manager singleton used for channel-scoped subscription state.


```solidity
address internal subscriptionManager
```


## Functions
### _getSubscriptionTokenStorage


```solidity
function _getSubscriptionTokenStorage() private pure returns (SubscriptionTokenStorage storage $);
```

### onlySubscriptionManager

Restricts callback entrypoints to the configured subscription manager.


```solidity
modifier onlySubscriptionManager() ;
```

### _onlySubscriptionManager

Validates caller is the configured subscription manager.


```solidity
function _onlySubscriptionManager() internal view;
```

### __SubscriptionModule_init

Initializes the subscription module and its parent upgradeable contracts.

Must be called inside the child contract's `initialize` / `reinitializer` function.


```solidity
function __SubscriptionModule_init(address _owner, string memory _tokenURI, address _subscriptionManager)
    internal
    onlyInitializing;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_owner`|`address`|Address that receives the `PLAN_MANAGER` role.|
|`_tokenURI`|`string`|Base URI used for ERC-1155 token metadata (appended with `/{id}.json`).|
|`_subscriptionManager`|`address`|Address of the singleton subscription manager.|


### setSubscriptionManager

Updates the singleton subscription manager reference.


```solidity
function setSubscriptionManager(address _subscriptionManager) external onlyRole(DEFAULT_ADMIN_ROLE);
```

### _setSubscriptionManager


```solidity
function _setSubscriptionManager(address _subscriptionManager) internal;
```

### plans

Returns the subscription plan for a given ID.


```solidity
function plans(uint8 planId) external view returns (SubscriptionPlan memory);
```

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


### _requireRoyaltyBackedPlans

Reverts when the channel carries a subscription plan but has no `ROYALTY_SHARE` supply.

Reads the channel's own ERC-1155 supply internally so it is valid both during channel
construction (proxy `initialize`) and afterwards — an external call back to a channel under
construction would hit a non-contract address. Callers gate this to plan-introducing paths so
plan-less asset-host / wrapper channels are left untouched — see V3C-3.


```solidity
function _requireRoyaltyBackedPlans() internal view;
```

### setPlanTokenURI

Sets metadata URI for the plan-mask token of a given plan id.

This is decoupled from plan mutation to keep channel bytecode lean.


```solidity
function setPlanTokenURI(uint8 planId, string calldata planTokenURI) external;
```

### subscribePlan

Subscribes `msg.sender` to a plan.


```solidity
function subscribePlan(uint8 planId, bytes calldata args) external payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`planId`|`uint8`|Plan identifier.|
|`args`|`bytes`|ABI-encoded subscription args for extensible workflow data.|


### unsubscribePlan

Cancels the caller's subscription to the given plan.


```solidity
function unsubscribePlan(uint8 planId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`planId`|`uint8`|Plan identifier.|


### hasActiveSubscription

Returns whether an account currently has an active subscription.


```solidity
function hasActiveSubscription(address account) public view virtual returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`||

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|`true` if the account has a non-expired subscription.|


### processSubscriptionState

Manager callback used to execute mint/payment flow on the channel.

Callable only by `subscriptionManager`.


```solidity
function processSubscriptionState(
    address subscriber,
    SubscriptionPlan calldata plan,
    uint256 subscriberCount,
    string calldata subscriptionTokenURI
) external payable onlySubscriptionManager;
```

### _processSubscription


```solidity
function _processSubscription(
    address subscriber,
    SubscriptionPlan memory plan,
    uint256 subscriberCount,
    string memory subscriptionTokenURI
) internal virtual;
```

### configureTokenOwnershipAccess

Configure token ownership access


```solidity
function configureTokenOwnershipAccess(TokenAccessThreshold[] calldata _input) external override;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_input`|`TokenAccessThreshold[]`|Array of token access thresholds|


### royaltyInfo

Computes the royalty distribution for a given sale price, proportional to each
holder's ROYALTY_SHARE balance. Rounding dust is assigned to the last holder.


```solidity
function royaltyInfo(uint256 _salePrice) public view returns (RoyaltyInfo[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_salePrice`|`uint256`|Total sale amount to distribute.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`RoyaltyInfo[]`|Array of `RoyaltyInfo` structs (receiver, amount).|


### _remapRoyaltyHoldings

Updates the `royaltyHolders` set after a ROYALTY_SHARE token transfer.

Removes `from` if their balance reaches zero; adds `to` if `amount > 0`.


```solidity
function _remapRoyaltyHoldings(address from, address to, uint256 amount) internal;
```

### hasTradeAccess

Determines whether `account` is allowed to trade the given token.

For ROYALTY_SHARE tokens, the caller must either hold shares or have an active subscription.


```solidity
function hasTradeAccess(address account, uint256 tkId) public view override returns (bool);
```

### tokenURI

Get the calculated value of the `tokenURI` by replacing `{id}` to the `tokenId` value.


```solidity
function tokenURI(uint256 tokenId) public view virtual returns (string memory);
```

### uri

See {IERC1155MetadataURI-uri}.
This implementation returns the concatenation of the `_baseURI`
and the token-specific uri if the latter is set
This enables the following behaviors:
- if `_tokenURIs[tokenId]` is set, then the result is the concatenation
of `_baseURI` and `_tokenURIs[tokenId]` (keep in mind that `_baseURI`
is empty per default);
- if `_tokenURIs[tokenId]` is NOT set then we fallback to `super.uri()`
which in most cases will contain `ERC1155._uri`;
- if `_tokenURIs[tokenId]` is NOT set, and if the parents do not have a
uri value set, then the result is empty.


```solidity
function uri(uint256 tokenId)
    public
    view
    override(ERC1155Upgradeable, ERC1155URIStorageUpgradeable)
    returns (string memory);
```

### _update

Overrides ERC-1155 `_update` to enforce exclusive-transfer restrictions
and keep the `royaltyHolders` set in sync after every ROYALTY_SHARE transfer.


```solidity
function _update(address from, address to, uint256[] memory ids, uint256[] memory values)
    internal
    virtual
    override(ERC1155Upgradeable, ERC1155SupplyUpgradeable);
```

### setPaymentProcessor

Restrict payment processor changes to admin role.


```solidity
function setPaymentProcessor(address _payProc) external override onlyRole(DEFAULT_ADMIN_ROLE);
```

### _refundOrCreditExcessNative

Attempts to refund excess native currency to `account`. If the direct
transfer fails (e.g. recipient is a contract without a receive function),
the amount is credited to `pendingRefunds` for later claim via `claimRefund()`.


```solidity
function _refundOrCreditExcessNative(address account, uint256 amount) internal;
```

### supportsInterface

Resolves the diamond-inheritance `supportsInterface` conflict.


```solidity
function supportsInterface(bytes4 interfaceId)
    public
    view
    virtual
    override(AccessControlUpgradeable, ERC1155Upgradeable, IERC165, TradeAccessRestriction)
    returns (bool);
```

### _normalizeTokenURIForStorage

Normalizes a token URI for ERC1155URIStorage base-URI concatenation.
If `tokenUri` starts with `ipfs://`, only the path part is stored so
the configured `ipfs://` base remains canonical.


```solidity
function _normalizeTokenURIForStorage(string memory tokenUri) internal pure returns (string memory);
```

## Errors
### InvalidSubscriptionManager
Thrown when an invalid manager address is provided.


```solidity
error InvalidSubscriptionManager(address manager);
```

### UnauthorizedSubscriptionManagerCaller
Thrown when callback entrypoints are called by non-manager contracts.


```solidity
error UnauthorizedSubscriptionManagerCaller(address caller);
```

### ChannelHasNoRoyaltyShares
A subscription plan cannot exist on a channel with zero `ROYALTY_SHARE` supply.

Royalty shares are minted only at channel creation and can never be minted afterward.
Without them `royaltyInfo` is empty and the deferred subscription payout reverts
`EmptyCommitError`, making the plan permanently unbuyable (V3C-3, INV-4). The guard is enforced
whenever a plan is introduced — at channel creation (`configureChannel`) and on later additions
(`bulkUpdatePlans`) — without over-rejecting plan-less asset-host / wrapper channels.


```solidity
error ChannelHasNoRoyaltyShares();
```

## Structs
### SubscriptionTokenStorage
**Note:**
storage-location: erc7201:elacity.drm.storage.SubscriptionModule.token


```solidity
struct SubscriptionTokenStorage {
    /**
     * @dev Set of addresses that currently hold ROYALTY_SHARE tokens.
     */
    EnumerableSet.AddressSet royaltyHolders;
}
```

