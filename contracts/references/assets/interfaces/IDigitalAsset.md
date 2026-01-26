## IDigitalAsset

### DigitalAssetRegistered

```solidity
event DigitalAssetRegistered(address ledger, uint256 tokenId, address operator)
```

This event is triggered when a new protected asset is created, this event doesn't concern
unencrypted content at all. This event is basically used for mapping an asset to its Operative contract

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| ledger | address | Address of the container - basically it's the channel where the asset if hosted on |
| tokenId | uint256 | The id of the new asset |
| operator | address | Address of the operative contract that handle the asset |

### AssetCreated

```solidity
event AssetCreated(address _to, uint256 _tokenId, string _tokenURI, uint16 _opType, address opContract)
```

This event is triggered when a new asset is created rgardless of whether it's encrypted or not.
This event is mostly for mapping ownership and other details as an asset

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _to | address | Address of the beneficiary |
| _tokenId | uint256 | ID of the new asset |
| _tokenURI | string | URI of the json-formatted metadata of the asset |
| _opType | uint16 | Type of the operative contract in charge, `0` for unencrypted asset |
| opContract | address | Address of the Operative contract in charge of the asset |

### mint

```solidity
function mint(string _uri, uint16 opType, bytes opRawData, bytes sellRawData) external payable
```

Create a new digital asset, generates the operative contract
with initial tokens distribution and put it on sale

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _uri | string | string memory - Token URI |
| opType | uint16 | uint16 - Operative type, can be 0: free, 1: Buy-Play, 2: Buy-Play-Sell. refers to OP_TYPE_* constants for complete set of the type |
| opRawData | bytes | bytes calldata - Operative raw data, "0x" to omit operative contract creation |
| sellRawData | bytes | bytes calldata - Sell raw data, "0x" to omit listing step |

### tokenURI

```solidity
function tokenURI(uint256 tokenId) external view returns (string)
```

Retrieve the URI of the json-formatted metadata of the asset

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| tokenId | uint256 | ID of the asset |

