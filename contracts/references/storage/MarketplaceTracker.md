## MarketplaceTracker

### TaxOverflowError

```solidity
error TaxOverflowError(uint256)
```

### Listing

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

### _get32BytesKey

```solidity
function _get32BytesKey(address op, uint256 tokenId) internal pure returns (bytes32)
```

### setListing

```solidity
function setListing(address op, uint256 tokenId, address _owner, uint256 qt, uint256 pricePerToken, address payToken) external
```

### getListing

```solidity
function getListing(address op, uint256 tokenId, address _owner) external view returns (uint256, uint256, address)
```

### sellersOf

```solidity
function sellersOf(address op, uint256 tokenId) external view returns (address[])
```

get list of sellers for a given token

### setOffer

```solidity
function setOffer(address op, uint256 tokenId, address _from, uint256 qt, uint256 pricePerToken, address payToken) external
```

### getOffer

```solidity
function getOffer(address op, uint256 tokenId, address _from) external view returns (uint256, uint256, address)
```

### offerersOf

```solidity
function offerersOf(address op, uint256 tokenId) external view returns (address[])
```

### setTaxInformation

```solidity
function setTaxInformation(uint16 _platformFee, address _feeRecipient) external
```

Update Tax settings

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _platformFee | uint16 | Units of tax to receive |
| _feeRecipient | address | Address of the tax recipient |

### taxInformation

```solidity
function taxInformation() external view returns (uint16, address)
```

