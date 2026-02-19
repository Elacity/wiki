## SystemTracker

This abstract contract is used to track the system contracts, especially the internal acknowledgement system

It should be mounted on `CoreStorage` contract.

### UnauthorizedAckError

```solidity
error UnauthorizedAckError(address caller)
```

