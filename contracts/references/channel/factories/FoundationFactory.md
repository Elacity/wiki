## ChannelFoundationFactory

Defines all the common members of a channel factory

### paymentProcessorFactory

```solidity
contract IPaymentProcessorFactory paymentProcessorFactory
```

### tradeGateway

```solidity
address tradeGateway
```

### constructor

```solidity
constructor(contract IPaymentProcessorFactory _ppf, address _tg) internal
```

### _setPaymentProcessor

```solidity
function _setPaymentProcessor(address channelAddress) internal
```

### _allowTransferOf

```solidity
function _allowTransferOf(address channel, address operator, uint256 tkId) internal
```

