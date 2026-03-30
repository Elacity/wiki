## IDigitalAsset

Interface for the digital asset contract.

### DigitalAssetRegistered

```solidity
event DigitalAssetRegistered(address ledger, uint256 tokenId, address operator)
```

Emitted when a protected asset is linked to an operative contract.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| ledger | address | Channel contract address hosting the asset. |
| tokenId | uint256 | Newly created asset id. |
| operator | address | Operative contract handling protected access. |

### AssetCreated

```solidity
event AssetCreated(address _to, uint256 _tokenId, string _tokenURI, uint16 _opType, address opContract)
```

Emitted when a new asset is minted, encrypted or unencrypted.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _to | address | Asset beneficiary. |
| _tokenId | uint256 | Asset identifier. |
| _tokenURI | string | Metadata URI of the json-formatted metadata of the asset. |
| _opType | uint16 | Operative type id (`0` for unencrypted assets). |
| opContract | address | Operative contract address, or zero when none is created. |

### mint

```solidity
function mint(string _uri, uint16 opType, bytes opRawData, bytes sellRawData) external payable
```

Mints a digital asset and optionally initializes its protection/trading flow.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _uri | string | Token metadata URI. |
| opType | uint16 | Operative type id. |
| opRawData | bytes | Encoded operative initialization data (`0x` to skip). |
| sellRawData | bytes | Encoded listing data (`0x` to skip). |

### tokenURI

```solidity
function tokenURI(uint256 tokenId) external view returns (string)
```

Returns metadata URI for a token.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| tokenId | uint256 | Asset id. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | string | URI string for the token metadata. |

