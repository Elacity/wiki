## MarketplaceTracker

This abstract contract is used to track the marketplace listings and offers. It should be mounted on
`CoreStorage` contract.

### TaxOverflowError

```solidity
error TaxOverflowError(uint256)
```

### InvalidFeeRecipient

```solidity
error InvalidFeeRecipient(address recipient)
```

### UnauthorizedTaxSetter

```solidity
error UnauthorizedTaxSetter(address caller)
```

### owner

```solidity
function owner() public view virtual returns (address)
```

Owner resolver expected from inheriting storage root (e.g., CoreStorage).

### Listing

Structure to hold listing details

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

```solidity
struct Listing {
  uint256 quantity;
  uint256 pricePerToken;
  address payToken;
}
```

### listings

```solidity
mapping(address => mapping(uint256 => mapping(address => struct MarketplaceTracker.Listing))) listings
```

map of listings
op address -> tokenId (ACCESS_TOKEN | ROYALTY | ...) -> owner -> Listing Details

### Offer

Structure to hold offer details

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

```solidity
struct Offer {
  uint256 quantity;
  uint256 pricePerToken;
  address payToken;
}
```

### offers

```solidity
mapping(address => mapping(uint256 => mapping(address => struct MarketplaceTracker.Offer))) offers
```

Map of offers for a given token (op -> tokenId -> offerer -> Offer)

### sellersOf

```solidity
function sellersOf(address op, uint256 tokenId) external view returns (address[])
```

Get list of sellers for a given token

### getOffer

```solidity
function getOffer(address op, uint256 tokenId, address _from) external view returns (uint256, uint256, address)
```

Get offer for a given token

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | address - Address of the Operative contract |
| tokenId | uint256 | uint256 - Token ID of the asset |
| _from | address | address - Address of the offerer |

### offerersOf

```solidity
function offerersOf(address op, uint256 tokenId) external view returns (address[])
```

Get list of offerers for a given token

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | address - Address of the Operative contract |
| tokenId | uint256 | uint256 - Token ID of the asset |

### taxInformation

```solidity
function taxInformation() external view returns (uint16, address)
```

Returns current platform fee settings.

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint16 | Fee in basis points over 1000. |
| [1] | address | Recipient of the platform fee. |

