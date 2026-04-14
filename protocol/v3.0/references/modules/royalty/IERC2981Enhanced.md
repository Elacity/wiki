# IERC2981Enhanced
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/modules/royalty/IERC2981Enhanced.sol)

**Inherits:**
IERC165

**Title:**
IERC2981Enhanced

This contract is extending ERC2981, which doesn't support multi royalties distribution,
it definee new way to store royalties, generally we process royalty payment from Operative contracts
that is supposed to be able retrieve these royalties distribution information


## Functions
### royaltyInfo


```solidity
function royaltyInfo(uint256 _salePrice) external view returns (RoyaltyInfo[] memory);
```

## Structs
### RoyaltyInfo

```solidity
struct RoyaltyInfo {
    address receiver;
    uint256 amount;
}
```

