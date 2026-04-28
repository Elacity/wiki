# Getting Started (v3.0)

Entry-point documents for understanding the current protocol model and migration context.

For Solidity-level behavior, consider [`Elacity/v3-drm-protocol`](https://github.com/Elacity/v3-drm-protocol) as the canonical source.

## Contents

- [Design Snapshot](design-proposal.md)
- [Ecosystem Overview](ecosystem-overview.md)
- [Security Analysis](security-analysis.md)

## Protocol Contract Addresses

{% tabs %}
{% tab title="Arbitrum Sepolia" %}
Source: `deployments/421614.json`

| Contract | Address |
| --- | --- |
| `CentralStorage` | [`0x961D93965EA749E1e0A9E96dde05E7C464c59a46`](https://sepolia.arbiscan.io/address/0x961D93965EA749E1e0A9E96dde05E7C464c59a46) |
| `AuthorityGateway` | [`0x5207439A56C16A6fFb02f1AF0321D79Cf037738f`](https://sepolia.arbiscan.io/address/0x5207439A56C16A6fFb02f1AF0321D79Cf037738f) |
| `RoyaltyTradeGateway` | [`0x308AB0599FCb255773959B994250B9A5b87Db689`](https://sepolia.arbiscan.io/address/0x308AB0599FCb255773959B994250B9A5b87Db689) |
| `ChannelFactory` | [`0x115f2Ab2a43A2f2D03b5e1cb6eBF6d65C52AdB23`](https://sepolia.arbiscan.io/address/0x115f2Ab2a43A2f2D03b5e1cb6eBF6d65C52AdB23) |
| `SubscriptionManager` | [`0x5f1b5D92e80adCB8B44283587585aDA5391Aff5f`](https://sepolia.arbiscan.io/address/0x5f1b5D92e80adCB8B44283587585aDA5391Aff5f) |
| `EventHub` | [`0x22b37b1eCDf33F1763beC30A167AC312c93A64e7`](https://sepolia.arbiscan.io/address/0x22b37b1eCDf33F1763beC30A167AC312c93A64e7) |
| `PublicChannelFactory` | [`0xC289D60aAe291e885BBc473d5124A5008Ca44C0a`](https://sepolia.arbiscan.io/address/0xC289D60aAe291e885BBc473d5124A5008Ca44C0a) |
| `PrivateChannelFactory` | [`0xaC99175f7474e51ACb1DFA4bCf49610c60d9d765`](https://sepolia.arbiscan.io/address/0xaC99175f7474e51ACb1DFA4bCf49610c60d9d765) |
| `MultiChannelFactory` | [`0x9bBf0696478D31e1A4f52e071bB68D0AEfEE8E27`](https://sepolia.arbiscan.io/address/0x9bBf0696478D31e1A4f52e071bB68D0AEfEE8E27) |
| `AssetFactory` | [`0xD60E24c47469ec5E82197caad055cF8866245141`](https://sepolia.arbiscan.io/address/0xD60E24c47469ec5E82197caad055cF8866245141) |
| `BuyableOperativeFactory` | [`0x203d8Cb8cbd4D1F238fdf0b9b4dC934E682633C4`](https://sepolia.arbiscan.io/address/0x203d8Cb8cbd4D1F238fdf0b9b4dC934E682633C4) |
| `BuyableSellableOperativeFactory` | [`0xb4Fd0F362D60C5323199D8bA5696B9268B4736f1`](https://sepolia.arbiscan.io/address/0xb4Fd0F362D60C5323199D8bA5696B9268B4736f1) |
{% endtab %}

{% tab title="Base" %}
Source: `deployments/8453.json`

| Contract | Address |
| --- | --- |
| `CentralStorage` | [`0x0C1EeA2A3361B80AC0e42179335dB536A951760b`](https://basescan.org/address/0x0C1EeA2A3361B80AC0e42179335dB536A951760b) |
| `AuthorityGateway` | [`0x09dBe796f40ECEffEAccf243c3d758C4c1d8D87D`](https://basescan.org/address/0x09dBe796f40ECEffEAccf243c3d758C4c1d8D87D) |
| `RoyaltyTradeGateway` | [`0xd02451BCE627EF476B8ee52Cf131C426f67dbcB2`](https://basescan.org/address/0xd02451BCE627EF476B8ee52Cf131C426f67dbcB2) |
| `ChannelFactory` | [`0xE1365ed47353De2F8A6a69E271e36650A9EE368F`](https://basescan.org/address/0xE1365ed47353De2F8A6a69E271e36650A9EE368F) |
| `SubscriptionManager` | [`0xb00456b57598006ef11d1F1678DcE68713eC897D`](https://basescan.org/address/0xb00456b57598006ef11d1F1678DcE68713eC897D) |
| `EventHub` | [`0x5a694A6d988354dca491fe0F6db7a6ef46b656c2`](https://basescan.org/address/0x5a694A6d988354dca491fe0F6db7a6ef46b656c2) |
| `PublicChannelFactory` | [`0xfcDffDd1cb844Fb3AC8c5d3477dF227E6E94ff8c`](https://basescan.org/address/0xfcDffDd1cb844Fb3AC8c5d3477dF227E6E94ff8c) |
| `PrivateChannelFactory` | [`0x6d0369f5AE83528CC8723027e5F219380d2F26A8`](https://basescan.org/address/0x6d0369f5AE83528CC8723027e5F219380d2F26A8) |
| `MultiChannelFactory` | [`0x2E8B108a60189af117F428A6827B3Bfb2e830931`](https://basescan.org/address/0x2E8B108a60189af117F428A6827B3Bfb2e830931) |
| `AssetFactory` | [`0x4c80A6209F16437f0dc4a98E3D43f08aeBF57765`](https://basescan.org/address/0x4c80A6209F16437f0dc4a98E3D43f08aeBF57765) |
| `BuyableOperativeFactory` | [`0xFbf39a097aa5577666e30de499e72120C8B3E82a`](https://basescan.org/address/0xFbf39a097aa5577666e30de499e72120C8B3E82a) |
| `BuyableSellableOperativeFactory` | [`0xd4FE224a71bF3C0c8F3075C4e5FB638C30517DfE`](https://basescan.org/address/0xd4FE224a71bF3C0c8F3075C4e5FB638C30517DfE) |
{% endtab %}

{% tab title="ESC" %}
(Not yet supported)
{% endtab %}
{% endtabs %}
