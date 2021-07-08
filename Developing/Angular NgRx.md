- [Redux Pattern](#redux-pattern)
- [Reducer](#reducer)
- [Dispatching an Action](#dispatching-an-action)
- [Subscribing to state changes](#subscribing-to-state-changes)
- [Redux Store Dev Tools](#redux-store-dev-tools)
- [Interfaces for strong typing of data in state](#interfaces-for-strong-typing-of-data-in-state)
- [Selectors](#selectors)
- [Action Creators](#action-creators)
- [Effects](#effects)
- [Create Barrel with index.ts](#create-barrel-with-indexts)
- [Other NgRx packages](#other-ngrx-packages)
- [Testing tools](#testing-tools)

### Redux Pattern

Use to manage application state. Application state is saved in store. 
All components subscribe to store in order to retrieve the state and any subsequent changes during their lifetime.

**Advantages**

 1. Centralized **immutable state**
 2. Performance (goes hand-in-hand with OnPush change detection strategy)
 3. Testability
 4. Tooling
 5. Component communication

**Principles**

 1. Single source of truth called the **store**
 2. State is read only and changes only by dispatching **actions**
 3. Changes are made using pure functions called **reducers**

### Reducer

Replaces the state with new state.
Responds to dispatched actions.
1 reducer per feature e.g. UserReducer, ProductReducer, ReviewReducer
Implement Actions with `on` functions (on handlers).

### Dispatching an Action

 1. It occurs in response to a user action.
 2. Inject the store in the constructor.
 3. Call the dispatch method of the store.
 4. Pass in the action to dispatch.

### Subscribing to state changes

Component must subscribe to state changes. Usually done in the ngOnInit lifecycle hook.
 
 ### Redux Store Dev Tools
 
 1. browser extension `Redux DevTools`
 2. @ngrx/store-devtools

### Interfaces for strong typing of data in state

Implement reducer specific interfaces in corresponding files.
Also implement state interface in a separate file e.g. app.state.ts 

### Selectors

Functions to query state data. Implement selectors in reducer files with methods`createSelector`, `createFeatureSelector`. The advantages from using selectors are:
* strongly typed API
* decouple the store from the components
* encapsulate complex data transformations
* reusable
* memoized (cached)

### Action Creators

Use to strongly type Actions. Utilize method `createAction` and provide action name and action data (optional) as function arguments. Define multiple actions for complex operations like loading data from a REST API (e.g. loadProducts, loadProductsSuccess, loadProductsError).

### Effects

@ngrx/effects 
Effects take an action, do some work and dispatch a new action. It's an Angular service.

Advantages:
 1. Keep components pure
 2. Isolate side effects
 3. Easier to test

**Using Effects for loading products**

 4. Inject the store
`constructor(private store: Store<State>){}`
 5. Call the dispatch method
 `this.store.dispatch(ProductActions.loadProducts());`
 6. Select state with selector
 `this.products$ = this.store.select(getProducts);`
 7. Add async pipe
 `*ngIf="products$ | async as products"`

**Building an Effect for updating a product**

 1. Build effect service and inject Actions
```
@Injectable()
export class ProductEffects {
	constructor(private actions$: Actions) {}
}
```
  2. Define a property
`updateProduct$ = createEffect(() => {`
  3. Build the effect
```
return this.actions$.pipe(
	ofType(ProductActions.updateProduct),
	concatMap( action =>
		this.productService.updateProduct(action.product).pipe(
			map(product => {roductActions.updateProductSuccess({ product }),
			catchError(error => of(ProductActions.updateProductFailure({ error })))
		)));
```
 
 ### Create Barrel with index.ts
Barrels are rollup exports from several modules into a single module. Add exports  in a index.ts file and create cleaner imports. With barrels  we have:
  1. public API for feature state module
  2. groups of Actions (PageActions, ApiActions) in separate files
  3. selectors and state interfaces in index.ts


### Other NgRx packages

* **@ngrx/entity** Helper functions for Create, Read, Update, Delete (CRUD) operations on entities. Reduces the code required to manage entities.
* **@ngrx/schematics** Set of schematics for generating NgRx related code with following CLI commands
	* ng generate store
	* ng generate action
	* ng generate reducer
	* ng generate effect
	* ng generate feature
	* ng generate container
	* ng generate entity
* **@ngrx/router-store** Connects the Angular router to store
* **@ngrx/data** Abstracts away the NgRx entity code. NO code for actions, action creators, reducers, selectors, effects is necessary.
* **@ngrx/component** Set of helpers to enable more fully reactive applications


### Testing tools

[Wallaby](https://wallabyjs.com/docs/index.html)


> Written with [StackEdit](https://stackedit.io/).
