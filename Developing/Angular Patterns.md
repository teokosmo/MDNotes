- [Design Patterns](#design-patterns)
  - [Components](#components)
    - [Presentational components](#presentational-components)
    - [Controller/Container components](#controllercontainer-components)
  - [File structure](#file-structure)

### Subscriptions

[SubSink](https://www.npmjs.com/package/subsink) package to handle subscriptions

### NgRx

[Redux DevTools](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en)

[NgRx-Data](https://github.com/johnpapa/angular-ngrx-data) package to reduce boilerplate code. Merged to NgRx as of version 8
[NgRx docs](https://ngrx.io/guide/data)
[NgRx repo](https://github.com/ngrx/platform)

### Preloading Javascript

in app-routing.module.ts utilize property preloadingStrategy to
* preload all modules
* preload specific modules
* preload with specific logic (e.g. on menu item hover)

### Serverless

 `Azure Functions`

### Compound debugging

combine multiple configurations of launch.json

## Design Patterns

### Components
components should be small, simple and do 1 and only 1 thing 

#### Presentational components
Concerned with how things look.
Contain HTML & CSS rules.
NO dependencies on the rest of the app (services, store).
Emit events via @Outputs.
Receive data via @Inputs.
May contain other components.

#### Controller/Container components
Concerned with how things work.
Little to no HTML and CSS rules.
Have injected dependencies (services, store).
Are stateful and specify how data is loaded or changed.
Top level routes.
May contain other components.

### File structure
as flat as possible

> Written with [StackEdit](https://stackedit.io/).