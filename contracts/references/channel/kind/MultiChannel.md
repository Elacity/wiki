## MultiChannel

This channel kind wraps another channels in a way that, all subscriptions
to this channel should have access to underlined channels that it wraps

### name

```solidity
string name
```

### constructor

```solidity
constructor() public
```

### initialize

```solidity
function initialize(address _authority, address _registry, address creator, string _name, string _tokenURI, bytes _data) public
```

### configureChannel

```solidity
function configureChannel(bytes data) internal
```

configure the channel by parsing and applying setup

The input for `MultiChannel` type is formatted as following `(ShareInput[], SubscriptionPlan[], address[])`

The flow is designed as following:
 - setup initial distribution of the royalty tokens
 - setup subscription plans
 - for each channels, bind them into this multi-channel contract

Here is an example of data formatting in javascript using ethersjs

```javascript
ethers.utils.defaultAbiCoder.encode(
  [
    'tuple(address,uint256)[]',
    'tuple(uint8,address,uint256,uint256,bool)[]',
    'address[]',
    'tuple(address,uint256)[]',
  ],
  [
    [
       // Royalties distribution
       ['0x02...12e5', 800],
       ['0x52...D17B', 200]
    ], [
       // 1 plan to create
       [0, ethers.constants.AddressZero, BigNumber.from(1).pow(18), 2592000, true]
    ], [
       // 2 address to map
       '0x09...C52D',
       '0x14...6fdf'
    ],
    []
  ]
);
```

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| data | bytes | raw input to configure the channel |

### wrapChannel

```solidity
function wrapChannel(address addr) external
```

Register a new channel into a wrapper

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| addr | address | Channel to add |

### supportsInterface

```solidity
function supportsInterface(bytes4 interfaceId) public view returns (bool)
```

