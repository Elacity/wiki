## SmartAccountUtils

_Library providing utilities for smart account compatibility
without relying on runtime bytecode-size checks._

### getSender

```solidity
function getSender() internal view returns (address sender)
```

_Resolve the caller identity for signatures/actions.
If the direct caller exposes an `owner()` view, that owner is treated as the
effective signer; otherwise falls back to `tx.origin`._

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| sender | address | Effective sender for compatibility flows. |

### _checkContractOwner

```solidity
function _checkContractOwner(address ownedContract) internal view returns (bool, address)
```

