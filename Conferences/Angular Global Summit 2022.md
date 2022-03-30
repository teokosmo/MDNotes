# Angular Global Summit 2022

Agenda: <https://geekle.us/schedule/angular22>

## DAY 1

<https://www.youtube.com/watch?v=Ytj-Z3HdCOM>

### NgRx

Redux pattern:

* Store
* Actions
* Selectors

### Building PWA apps

To create an Angular PWA application use [Workbox](https://developers.google.com/web/tools/workbox)
Use Service Worker (SW) & Cache Storage (cache).

Strategies to follow when building an Angular PWA app:

* CacheFirst
* CacheOnly
* NetworkFirst
* NetworkOnly
* StaleWhileRevalidate

### Building web3 apps

Blockchain is a decentralized database

**Blockchain types**

1. Public Blockchains
   * Bitcoin
   * Ethereum
2. Private Blockchains
    * Hyperledger
3. Consortium Blockchains
    * Energy Web Foundation

**Technical Stack**

1. Smart Contracts (Backend)
    * Solidity
    * Clarity
    * Rust
    * C++
2. Web3 Scripts (Frontend)
    * Javascript (web3.js, ethers.js)
    * Python
    * Rust
    * Go
3. API Calls

**Web3 Libraries**

* Web3.js
* Ethers.js
* NFT-aggregator
* Price-indexer
* WalletConnect

**Defi & NFTs**

[CoinmarketCap](https://coinmarketcap.com/)

### Building web apps with accessibility in focus

* have screen reader (sr) read your web app
* display elements only in sr with class `sr-only`
* use data attributes `aria-label`, `aria-labelledby`
* check your web app in terms of accessibility with `Google Lighthouse`

### Build services for retrieving data with NSwag

NSwag is a Swagger/OpenAPI toolchain

### Angular components structure

When building components structure have in mind the following principles:

* Single Responsibility
* Reusability
* Don't repeat yourself (DRY)
* Single level of abstraction

### Static Site Generation (SSG)

Angular by default uses Client Side Rendering (CSR).
Server Side Rendering (SSR) is SEO friendly.
For SSG a static file server (CDN) is used e.g. [Puppeteer](https://github.com/puppeteer/puppeteer)

To build static content for Angular app, use [Scully](https://scully.io/). Scully finds out all Angular routes and generates respective html files.

## DAY 2


### NgRx

### CSS new stuff

1. pseudo selector [where](https://developer.mozilla.org/en-US/docs/Web/CSS/:where)
2. property [direction](https://developer.mozilla.org/en-US/docs/Web/CSS/direction)
3. property [writing-mode](https://developer.mozilla.org/en-US/docs/Web/CSS/writing-mode)
4. [margin-inline](https://developer.mozilla.org/en-US/docs/Web/CSS/margin-inline)

### Angular workspace/libraries

**NG Workspace**

Create apps and libraries in Angular workspace in a Monorepo.

In a Monorepo all libraries and apps have a single version.

**Nx**

### Dockerize Angular app

Run docker with **multi-stage builds**.

Consider security by running docker with a user (e.g. node) instead of root.


## Tips

1. use `ng add` to install an npm package in angular app

## Useful Links

1. [nodejs containerization](https://snyk.io/blog/10-best-practices-to-containerize-nodejs-web-applications-with-docker/)
2. [CSS architecture](https://www.youtube.com/watch?v=6pUYZjEqdvg)
