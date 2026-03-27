## IIPRepresentation

Interface for Intellectual Property Representation

_see [https://eips.ethereum.org/EIPS/eip-5553](https://eips.ethereum.org/EIPS/eip-5553)_

### MetadataChanged

```solidity
event MetadataChanged(address byAddress, string oldURI, string oldFileHash, string newURI, string newFileHash)
```

Event to be triggered whenever metadata URI is changed

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| byAddress | address | the addresses that triggered this operation |
| oldURI | string | the URI to the old metadata file before the change |
| oldFileHash | string | the hash of the old metadata file before the change |
| newURI | string | the URI to the new metadata file |
| newFileHash | string | the hash of the new metadata file |

### changeMetadataURI

```solidity
function changeMetadataURI(string _newUri, string _newFileHash) external
```

Called with the new URI to an updated metadata file

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _newUri | string | - the URI pointing to a metadata file (file standard is up to the implementer) |
| _newFileHash | string | - The hash of the new metadata file for future reference and verification |

### royaltyPortionTokens

```solidity
function royaltyPortionTokens() external view returns (address[])
```

Retrieve roylty portion based on token ownership

_i.e implementing ERC5501 (IRoyaltyInterestToken interface)_

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address[] | array of addresses of ERC20 tokens representing royalty portion in the IP |

### ledger

```solidity
function ledger() external view returns (address)
```

_i.e., a registry or registrar, to be implemented in the future_

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address | the address of the contract or EOA that initialized the IP registration |

### metadataURI

```solidity
function metadataURI() external view returns (string)
```

retrieve the metadata URI of the contract

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | string | the URI of the current metadata file for the II P |

