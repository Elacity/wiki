## IERC2981Enhanced

This contract is extending ERC2981, which doesn't support multi royalties distribution,
it definee new way to store royalties, generally we process royalty payment from Operative contracts
that is supposed to be able retrieve these royalties distribution information

### RoyaltyInfo

```solidity
struct RoyaltyInfo {
  address receiver;
  uint256 amount;
}
```

### royaltyInfo

```solidity
function royaltyInfo(uint256 _salePrice) external view returns (struct IERC2981Enhanced.RoyaltyInfo[])
```

