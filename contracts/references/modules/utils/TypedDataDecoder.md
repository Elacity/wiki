## EIP712Domain

```solidity
struct EIP712Domain {
  string name;
  string version;
  uint256 chainId;
  address verifyingContract;
}
```

## EIP712DOMAIN_TYPEHASH

```solidity
bytes32 EIP712DOMAIN_TYPEHASH
```

## IPEntity

```solidity
struct IPEntity {
  bytes16 contentId;
  address ledger;
  uint256 tokenId;
}
```

## IPENTITY_TYPEHASH

```solidity
bytes32 IPENTITY_TYPEHASH
```

## LicenseRequest

```solidity
struct LicenseRequest {
  string entitlement;
  struct IPEntity entity;
}
```

## LICENSEREQUEST_TYPEHASH

```solidity
bytes32 LICENSEREQUEST_TYPEHASH
```

## EIP712Decoder

### recover

```solidity
function recover(bytes32 hash, bytes sig) internal pure returns (address)
```

_Recover signer address from a message by using their signature_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| hash | bytes32 | bytes32 message, the hash is the signed message. What is recovered is the signer address. |
| sig | bytes | bytes signature, the signature is generated using web3.eth.sign() |

### GET_EIP712DOMAIN_PACKETHASH

```solidity
function GET_EIP712DOMAIN_PACKETHASH(struct EIP712Domain _input) internal pure returns (bytes32)
```

### GET_IPENTITY_PACKETHASH

```solidity
function GET_IPENTITY_PACKETHASH(struct IPEntity _input) internal pure returns (bytes32)
```

### GET_LICENSEREQUEST_PACKETHASH

```solidity
function GET_LICENSEREQUEST_PACKETHASH(struct LicenseRequest _input) internal pure returns (bytes32)
```

