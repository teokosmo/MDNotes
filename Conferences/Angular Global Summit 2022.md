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

<https://www.youtube.com/watch?v=_BHkB9KxMBA>

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

[Ref](https://nx.dev/)

### Dockerize Angular app

Run docker with **multi-stage builds**.

Consider security by running docker with a privileged user (e.g. node) instead of root.

### Unit Testing of Angular Material CDK

**Component harness**

[Ref](https://material.angular.io/cdk/test-harnesses/overview)

Write ComponentHarness classes for your library to be used by the users of your library. For example Angualar Material library has a component harness implementation for each component so that we use this component harness when writing tests for our component that utilizes Angular Material components.

### Web Components

Use [Angular elements](https://angular.io/guide/elements) to convert any Angular component to a framework agnostic Web Component (custom element).

[github example](https://github.com/marcellkiss/angular-web-component-example)

### Micro Frontends

[Building Angular Micro Frontend with NgRx state sharing](https://itnext.io/building-angular-micro-frontend-with-ngrx-state-sharing-and-nx-cli-7e9af10ebd03)

### Webpack Module Federation

[documentation](https://webpack.js.org/concepts/module-federation/)

[npm package](https://www.npmjs.com/package/@angular-architects/module-federation)


### Angular performance

[RxAngular](https://opencollective.com/rx-angular)

**Network optimizations**

Add the following to script and link tags when requesting css & js files
* preconnect
* preload
* prefetch
* defer
* async

Add attribute: loading="lazy" in tags `img` & `iframe`. In a list of images do not add "lazy" on the first image.

**use RxAngular in components**

Use `rxFor` instead of `ngFor` and `rxIf` instead of `ngIf`.

Do not use async pipe in templates, just the observable.

**zone**

Drop zone library. Just use app.tick when routes rendering ends.

### Design Systems

Key features of a design system:

* Design Tokens
* Design Kit
* Components Library
* Dynamic Documentation

## Tips

1. use `ng add` to install an npm package in angular app
2. Use Chrome's user flows for performance testing. Check [here](https://www.debugbear.com/blog/chrome-devtools-user-flow-recorder)

## Useful Links

1. [nodejs containerization](https://snyk.io/blog/10-best-practices-to-containerize-nodejs-web-applications-with-docker/)
2. [CSS architecture](https://www.youtube.com/watch?v=6pUYZjEqdvg)
3. [Svelte](https://svelte.dev/)
