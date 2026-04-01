---
description: Integrate and build a decentrelised DRM system with Ela.city
icon: code
---

# Elacity Developer Platform

<h2 align="center">Getting Started</h2>

<p align="center">Start your integration journey here!</p>

<p align="center"><a href="https://app.gitbook.com/s/3XvtoAOLz7xBoGxBi0rw/" class="button primary">Documentation</a> <a href="https://js-sdk-demo-latest.onrender.com/" class="button secondary">View Demo</a></p>

<table data-view="cards"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="image">Cover image</th></tr></thead><tbody><tr><td><i class="fa-cart-shopping-fast">:cart-shopping-fast:</i></td><td><strong>Typescript Support</strong></td><td>Integrate dDRM on your web-based application and marketplace.</td><td><a href="https://app.gitbook.com/o/4MuPhoLmRoszPOWBxvkU/s/x8iBADAyrABxD01kmAEw/">https://app.gitbook.com/o/4MuPhoLmRoszPOWBxvkU/s/x8iBADAyrABxD01kmAEw/</a></td><td data-object-fit="cover"><a href=".gitbook/assets/Libweb3 2.png">Libweb3 2.png</a></td></tr><tr><td><i class="fa-hexagon-nodes-bolt">:hexagon-nodes-bolt:</i></td><td><strong>Access Economy Protocol</strong></td><td>Learn more about the protocol built and deployed on EVM-based network</td><td><a href="https://app.gitbook.com/o/4MuPhoLmRoszPOWBxvkU/s/x8iBADAyrABxD01kmAEw/">https://app.gitbook.com/o/4MuPhoLmRoszPOWBxvkU/s/x8iBADAyrABxD01kmAEw/</a></td><td><a href=".gitbook/assets/Elacity Labs.png">Elacity Labs.png</a></td></tr><tr><td><i class="fa-terminal">:terminal:</i></td><td><strong>API reference</strong></td><td>Browse, test, and implement APIs.</td><td><a href="https://app.gitbook.com/o/4MuPhoLmRoszPOWBxvkU/s/NCW8PvkrgTSU6PMsWsDu/">https://app.gitbook.com/o/4MuPhoLmRoszPOWBxvkU/s/NCW8PvkrgTSU6PMsWsDu/</a></td><td><a href=".gitbook/assets/Flint 2.jpg">Flint 2.jpg</a></td></tr></tbody></table>



{% columns %}
{% column %}
#### Friendly integration

Setting up the API lies on specific network you are intended to work with.&#x20;

Interact with API REST and Graphql through \`@elacity-js/api\` and leverage on realtime-cached data from network activities



<a href="https://app.gitbook.com/s/3XvtoAOLz7xBoGxBi0rw/api-layer" class="button primary" data-icon="rocket-launch">Get started</a> <a href="https://app.gitbook.com/s/3XvtoAOLz7xBoGxBi0rw/contracts-evm-compatible" class="button secondary" data-icon="hexagon-nodes-bolt">Protocol</a>
{% endcolumn %}

{% column %}
{% code title="index.js" overflow="wrap" %}
```javascript
// Import statement
import { ElacityClient, type NFTItem } from '@elacity-js/api';
import { presets } from '@elacity-js/api/query';

// Initialize the client
const client = new ElacityClient({
  chainId: 8453, // Base
});

// Get list of NFTs based on preset query
const items = await client.nfts.fetchItems(...presets.buyNow);

```
{% endcode %}
{% endcolumn %}
{% endcolumns %}

#### Keep on track of changes

Get always on track of all of the changes overall the system for a better integration experience

<a href="https://app.gitbook.com/s/NvhEwQZWtWjwRyiY2sTp/" class="button primary" data-icon="book-open">Changelog</a>&#x20;

<h2 align="center">Need help? Join the community</h2>

<p align="center">Join our Telegram community or contribute to many of our open source projects in Github</p>

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th></th><th></th><th data-hidden data-card-cover data-type="image">Cover image</th></tr></thead><tbody><tr><td><i class="fa-telegram">:telegram:</i></td><td><strong>Join the community</strong></td><td>Join our Telegram community to post questions, get help, and share resources to like-minded people.</td><td><a href="https://t.me/elacity" class="button secondary">Join @elacity</a></td><td><a href=".gitbook/assets/Directory.png">Directory.png</a></td></tr><tr><td><i class="fa-github">:github:</i></td><td><strong>GitHub</strong></td><td>Head to our GitHub repository to learn more and/or contribute via PRs.</td><td><a href="https://github.com/Elacity" class="button secondary">Submit a PR</a></td><td><a href=".gitbook/assets/Welcome to Elacity compressed.png">Welcome to Elacity compressed.png</a></td></tr></tbody></table>
