## TokenIdGenerator

Generates pseudo-random token identifiers from user-provided seeds.

**Basic Usage**

```solidity
contract TokenRegistry {
   using TokenIdGenerator for string;
   error OutOfBound(bytes32 value);

   function mintNFT(string memory tokenURI) external {
     bytes32 _tokenId = _tokenURI.derivateId();
     if (_tokenId <= tokenSpaceProhibited) {
       revert OutOfBound(_tokenId);
     }
     // ... mint statement here with the predefined `tokenId`
   }
}
```

### derivateId

```solidity
function derivateId(bytes32 seed) public view returns (bytes32)
```

Generates a pseudo-random bytes32 ID

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| seed | bytes32 | A user-provided seed for added randomness |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bytes32 | A pseudo-random bytes32 ID |

### derivateId

```solidity
function derivateId(string seed) public view returns (bytes32)
```

Generates a pseudo-random bytes32 ID using a string as a seed

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| seed | string | A user-provided string seed for added randomness |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bytes32 | A pseudo-random bytes32 ID |

## ConstraintedTokenId

### generateTokenId

```solidity
function generateTokenId(string _tokenURI) external view returns (uint256)
```

Generates a constrained token id from a token URI seed.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _tokenURI | string | Seed string used to derive the token id. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Token id in allowed range, or `0` when result falls in prohibited space. |

