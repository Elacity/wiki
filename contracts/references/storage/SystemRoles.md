# SystemRoles
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/storage/SystemRoles.sol)

**Title:**
SystemRoles

Canonical contract capability role bitmasks used by protocol storage guards.

Roles are represented as bitmap flags and can be OR-composed for multi-capability assignment.


## State Variables
### ROLE_ASSET_REGISTRAR
Grants permission to orchestrate asset registration flows.


```solidity
uint256 internal constant ROLE_ASSET_REGISTRAR = 1 << 0
```


### ROLE_IP_REGISTRAR
Grants permission to mutate IP binding/operator registries.


```solidity
uint256 internal constant ROLE_IP_REGISTRAR = 1 << 1
```


### ROLE_MARKET_WRITER
Grants permission to write marketplace listings/offers.


```solidity
uint256 internal constant ROLE_MARKET_WRITER = 1 << 2
```


### ROLE_WRAPPER_REGISTRAR
Grants permission to register channel wrapper topology entries.


```solidity
uint256 internal constant ROLE_WRAPPER_REGISTRAR = 1 << 3
```


### ROLE_TRANSFER_AUTHORIZER
Grants permission to bypass strict owner transfer checks where explicitly enabled.


```solidity
uint256 internal constant ROLE_TRANSFER_AUTHORIZER = 1 << 4
```


