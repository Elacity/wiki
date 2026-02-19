## IMarketplaceTracker

Tracks listings, offers, participants, and marketplace fee settings.

### listings

```solidity
function listings(address op, uint256 tokenId, address owner) external returns (uint256, uint256, address)
```

Returns raw listing fields from storage.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | Operative contract address. |
| tokenId | uint256 | Asset token id. |
| owner | address | Listing owner/seller. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Quantity, unit price, and payment token address. |
| [1] | uint256 |  |
| [2] | address |  |

### sellersOf

```solidity
function sellersOf(address op, uint256 tokenId) external view returns (address[])
```

Returns all sellers with listing state for an asset.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | Operative contract address. |
| tokenId | uint256 | Asset token id. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address[] | Seller address array. |

### getListing

```solidity
function getListing(address op, uint256 tokenId, address owner) external view returns (uint256, uint256, address)
```

Returns listing details for a specific seller.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | Operative contract address. |
| tokenId | uint256 | Asset token id. |
| owner | address | Listing owner/seller. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Quantity, unit price, and payment token address. |
| [1] | uint256 |  |
| [2] | address |  |

### offers

```solidity
function offers(address op, uint256 tokenId, address owner) external returns (uint256, uint256, address)
```

Returns raw offer fields from storage.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | Operative contract address. |
| tokenId | uint256 | Asset token id. |
| owner | address | Offerer address. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Quantity, unit price, and payment token address. |
| [1] | uint256 |  |
| [2] | address |  |

### offerersOf

```solidity
function offerersOf(address op, uint256 tokenId) external view returns (address[])
```

Returns all offerers with offer state for an asset.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | Operative contract address. |
| tokenId | uint256 | Asset token id. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address[] | Offerer address array. |

### getOffer

```solidity
function getOffer(address op, uint256 tokenId, address owner) external view returns (uint256, uint256, address)
```

Returns offer details for a specific offerer.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | Operative contract address. |
| tokenId | uint256 | Asset token id. |
| owner | address | Offerer address. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Quantity, unit price, and payment token address. |
| [1] | uint256 |  |
| [2] | address |  |

### taxInformation

```solidity
function taxInformation() external view returns (uint16, address)
```

Returns marketplace fee settings.

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint16 | Platform fee in basis points and fee recipient address. |
| [1] | address |  |

