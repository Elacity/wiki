## IMarketplaceTracker

### listings

```solidity
function listings(address, uint256, address) external returns (uint256, uint256, address)
```

### setListing

```solidity
function setListing(address op, uint256 tokenId, address owner, uint256 qt, uint256 pricePerToken, address payToken) external
```

_keep on track of this issue https://github.com/ethereum/solidity/issues/6337
we need to define Listing here, not in the implementation_

### sellersOf

```solidity
function sellersOf(address op, uint256 tokenId) external view returns (address[])
```

### getListing

```solidity
function getListing(address op, uint256 tokenId, address owner) external view returns (uint256, uint256, address)
```

### offers

```solidity
function offers(address, uint256, address) external returns (uint256, uint256, address)
```

### setOffer

```solidity
function setOffer(address op, uint256 tokenId, address _from, uint256 qt, uint256 pricePerToken, address payToken) external
```

### offerersOf

```solidity
function offerersOf(address op, uint256 tokenId) external view returns (address[])
```

### getOffer

```solidity
function getOffer(address op, uint256 tokenId, address owner) external view returns (uint256, uint256, address)
```

### taxInformation

```solidity
function taxInformation() external view returns (uint16, address)
```

### setTaxInformation

```solidity
function setTaxInformation(uint16 _platformFee, address _feeRecipent) external
```

