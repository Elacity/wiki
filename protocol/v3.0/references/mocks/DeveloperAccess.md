# DeveloperAccess
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/bc1f2ea3fd5d8b703627a7946e7d5fe7fb13f047/contracts/mocks/DeveloperAccess.sol)

**Inherits:**
Initializable, ERC721Upgradeable, ERC721URIStorageUpgradeable, ERC721BurnableUpgradeable, OwnableUpgradeable


## Functions
### constructor

**Note:**
oz-upgrades-unsafe-allow: constructor


```solidity
constructor() ;
```

### initialize


```solidity
function initialize(address initialOwner) public initializer;
```

### _baseURI


```solidity
function _baseURI() internal pure override returns (string memory);
```

### safeMint


```solidity
function safeMint(address to, string memory uri) public onlyOwner returns (uint256 tokenId);
```

### tokenURI


```solidity
function tokenURI(uint256 tokenId)
    public
    view
    override(ERC721Upgradeable, ERC721URIStorageUpgradeable)
    returns (string memory);
```

### supportsInterface


```solidity
function supportsInterface(bytes4 interfaceId)
    public
    view
    override(ERC721Upgradeable, ERC721URIStorageUpgradeable)
    returns (bool);
```

## Errors
### EmptyURI

```solidity
error EmptyURI();
```

