# Node.js Global Summit 2023

[Agenda](https://geekle.us/schedule/nodejs23)

## Platformatic DB

<https://oss.platformatic.dev/>

Node.js framework that relies on [Fastify](https://www.fastify.io/) plugins.
Consider dropping ORM related frameworks like [NestJS](https://nestjs.com/) in favor of Platformatic DB.

## Build App with AirTable, Next.js, React Hooks

Speaker <https://loige.co/>

Slides <https://slides.com/lucianomammino/building-an-invite-only-microsite-with-nextjs-airtable-global-summit-for-nodejs>

Node.js Book <https://www.nodejsdesignpatterns.com/>

## Node.js execution


### V8 engine

compile and runs js files in a single thread

### libuv

Library used by Node.js for file system operations.

Uses Thread Pool to perform these operations.

### Event Loop

## InfluxData

InfluxData empower developers to build real-time IoT, analytics, and cloud applications.
Focus on the things you do best at with InfluxData. Get started for free at <https://www.influxdata.com/influxcloud-trial/>

Build an IoT platform with InfluxDB, Node.js and React.

### Direct Write into InfluxDB

Send data via POST requests using `line protocol`

### InfluxDB js client

<https://github.com/influxdata/influxdb-client-js/tree/master/examples>

## E2E Testing with Puppeteer

Puppeteer is a framework (or Node.js library) built by Google, <https://pptr.dev/>

## Functional programming in Node.js

### Pure functions

* always return value
* same input always return the same output
* does not change the value of input

### Immutability

no change in existing object/variable, create new ones instead

### Disciplined state

rely on pure functions and immutability to generate new values

### High Order functions

functions that take other functions as inputs

### Functional programming Advantages

* fewer bugs due to immutable state of the data
* pure functions are vey easy to test
* enhanced code readability
* function signatures are more trusted and easier to understand
* concurrency is more easily kept safe

### Javascript

functions are first-class objects ... (check MDN for more)

Array manipulation methods (high order functions)
Expression statements (ternary operator)
Method chaining

## Javascript Trends

Speaker <https://www.codewonders.dev/>

### Serverless Architecture

AWS lamda & Google Cloud functions

### Rise of Typescript


### Functional programming

popular libraries

* Lodash
* Ramda
* Functional-Redux

### GraphQL

query language for APIs

### Performance and scalability

#### Server Side Rendering (SSR)

* Next.js
* Nuxt.js

#### Code splitting & lazy loading

#### Performance tools

* Lighthouse

## Use TypeScript with Node.js

Node.js frameworks written in TypeScript

* NestJS
* TypeORM
* RxJS

## Web Streams in Node.js

Web Streams API & Fetch API in Node v18

<https://developer.chrome.com/articles/fetch-streaming-requests/>

### Readable Stream 

Push source

Pull source

### Writable Stream

### Transform Stream

data transformation between read and write streams

### Piping

data from one stream are piped to another stream

### Back pressure

### Teeing

turn a single readable stream into 2 independent streams

## Node.js & PWA

Speaker <in/sdkdeepa>

<https://sdkdeepa-nodejs2023.netlify.app/#60>

Create mobile or even desktop apps by creating PWA apps

necessary files for PWA

* manifest
* sw.js (service worker)

### manifest file
json file that tells the browser what to do when the app is installed

### service worker

executing in separate thread

First service worker must be registered by javascript code running in main thread

Features
* load content offline
* push notifications
* background sync

Validate PWA app with chrome extension Lighthouse, `Run Audit`.

Demo: <https://www.youtube.com/watch?v=CcxvA4iJRNQ&t=1s>

## Cypress tests

Speaker <https://robrich.org/presentations/>

Source <https://github.com/robrich/gaining-confidence-cypress-tests>



## Build CLI tools with Node.js

### Commander.js

Node.js tool that makes it easy to define commands and options

### Inquirer.js

Node.js package for creating interactive command-line prompts to guide user through the process of using the tool and to also validate the user input