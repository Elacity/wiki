## IPTracker

### IPIdentity

_Identification of a digital asset (IP) within the core ledger_

```solidity
struct IPIdentity {
  address channel;
  uint256 tokenId;
}
```

### operator

```solidity
mapping(address => mapping(uint256 => address)) operator
```

Collection -> Token ID -> op address

### registerDigitalAsset

```solidity
function registerDigitalAsset(address channel, uint256 tokenId, address op) external
```

Regsiter a Digital Asset item and bind it to an Operative contract

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| channel | address | address - Address of the NFT contract that holds registy of assets |
| tokenId | uint256 | uint256 - Token ID of the asset |
| op | address | address - Address of operative contract |

