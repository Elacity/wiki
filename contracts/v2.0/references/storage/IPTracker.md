## IPTracker

Tracks the operative contract responsible for each `(channel, tokenId)` asset pair.

### IPIdentity

Canonical identifier for a tracked digital asset.

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

Channel => tokenId => operative contract address.

