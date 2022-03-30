# Angular Global Summit 2022

Agenda: <https://geekle.us/schedule/angular22>

## DAY 1

<https://www.youtube.com/watch?v=Ytj-Z3HdCOM>

### NgRx

Redux pattern:

* Store
* Actions
* Selectors

### PWA

To create an Angular PWA application use [Workbox](https://developers.google.com/web/tools/workbox)
Use Service Worker (SW) & Cache Storage (cache).

Strategies to follow when building an Angular PWA app:

* CacheFirst
* CacheOnly
* NetworkFirst
* NetworkOnly
* StaleWhileRevalidate

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

### Tips

1. use `ng add` to install an npm package in angular app
