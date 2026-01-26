## ILicenseProvider

### IPRegistered

```solidity
event IPRegistered(address ledger, bytes16 contentId, bytes32 ipHash)
```

### License

```solidity
struct License {
  address issuer;
  address toAddress;
  struct IPEntity entity;
  bytes[] key;
}
```

### registerIPWithKey

```solidity
function registerIPWithKey(address ledger, bytes16 _contentId, bytes _ipKey) external returns (bytes32, bytes)
```

### acquireLicense

```solidity
function acquireLicense(bytes payload) external view returns (bytes4, bytes)
```

