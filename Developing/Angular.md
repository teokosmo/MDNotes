- [Angular services](#angular-services)
  - [Angular Dependency Injection (DI)](#angular-dependency-injection-di)
- [Component lifecycle hooks](#component-lifecycle-hooks)
- [RxJS](#rxjs)
  - [Observables](#observables)
  - [Operators](#operators)
  - [Promise vs Observable](#promise-vs-observable)
- [Directives](#directives)
  - [Angular Built-in](#angular-built-in)
  - [Custom](#custom)
- [Pipes](#pipes)
  - [Angular Built-in](#angular-built-in-1)
  - [Custom](#custom-1)
- [Forms](#forms)
  - [Template-driven forms, docs](#template-driven-forms-docs)
  - [Reactive forms](#reactive-forms)
- [template](#template)
  - [Safe Navigation Operator (?.)](#safe-navigation-operator-)
- [Binding](#binding)
  - [Interpolation](#interpolation)
  - [Property Binding](#property-binding)
  - [Event Binding](#event-binding)
  - [Class Binding](#class-binding)
  - [2-way Binding](#2-way-binding)
- [Component Communication](#component-communication)
- [ViewChild](#viewchild)
- [View Encapsulation](#view-encapsulation)
- [Content Projection](#content-projection)
- [OnPush Change Detection Strategy](#onpush-change-detection-strategy)
- [Routing](#routing)
  - [RoutingGuards](#routingguards)
  - [pre-loading data](#pre-loading-data)
  - [lazy loading](#lazy-loading)
- [Compilers](#compilers)
  - [Just in Time (JIT)](#just-in-time-jit)
  - [Ahead of Time (AOT)](#ahead-of-time-aot)
  - [Angular Ivy](#angular-ivy)
- [Server-side rendering (SSR) with Angular Universal](#server-side-rendering-ssr-with-angular-universal)
- [ETSS WebTrader](#etss-webtrader)
  - [Architecture](#architecture)
  - [Utilize OAuth2 protocol for authentication](#utilize-oauth2-protocol-for-authentication)

### Angular services

Single instance in the app (Singleton).
```
@Injectable({
	providedIn:  'root'
})
```

Module based instance i.e. there is an object instance only when the module is active.
`@Injectable()`
In Component use property `providers`
```
@Component({
	selector:  'app-markets',
	providers: [MarketsModuleService]
})

export class MarketsComponent
```
#### Angular Dependency Injection (DI)

[Reference](https://angular.io/guide/dependency-injection)


### Component lifecycle hooks

[Reference](https://angular.io/guide/lifecycle-hooks)

* ngOnChanges
* ngOnInit
* ngDoCheck
* ngAfterContentInit
* ngAfterContentChecked
* ngOnDestroy


### RxJS

#### Observables

* **Subject** Subscribers are notified as soon as method next() is invoked
* **BehaviorSubject** Subscribers are notified on subscribe with the current value of the observable and as soon as method next() is invoked
* **ReplaySubject** As in Subject & BehaviorSubject subscribers are notified as soon as method next() is invoked. But it has also the ability to store multiple old values and “replay” them to new subscribers. 
* **AsyncSubject** Only the last value of the Observable execution is sent to its subscribers, and only when the execution completes

#### Operators

* **switchMap** Cancels the current subscription/request and can cause race condition. Use for get requests or cancelable requests like searches.
* **concatMap** Runs subscriptions/requests in order and is less performant. Use for get, post and put requests when order is important.
* **mergeMap** Runs subscriptions/requests in parallel. Use for get, put, post and delete methods when order is NOT important.
* **exhaustMap** Ignores all subsequent subscriptions/requests until it completes. Use for login when you do not want more requests until the initial one is complete.

#### Promise vs Observable

|Promise| Observable |
|-------|--|
|Provides a single future value|Emits multiple values over time|
|Not lazy|Lazy (emits values only when subscribe() is invoked)|
|Not cancellable|Cancellable|

### Directives

#### Angular Built-in

* Structural Directives
	* *ngIf
	**ngif vs [hidden]**. If ngIf returns false the contained HTML is not appended in the DOM. With DOM property `hidden` element is not displayed but contained in the DOM.
	* *ngFor
trackBy
* Form directives
	* [ngForm](https://angular.io/api/forms/NgForm)
	* [ngSubmit](https://angular.io/api/forms/NgForm#ngSubmit)
	* [ngModel](https://angular.io/api/forms/NgModel)
* Common directives
	* [ngSwitch](https://angular.io/api/common/NgSwitch)
	* [ngClass](https://angular.io/api/common/NgClass) 
	* [ngStyle](https://angular.io/api/common/NgStyle)

#### Custom
```
@Directive({
	selector:  '[appCombinedOrdersMouseover]'
})

export  class  CombinedOrdersCellMouseoverDirective {
	
	@Input('appCombinedOrdersMouseover') orderMTUData: IOrderMTUData;
	@Input() columnIndex: number;
	@Input() gridGroupKey: string;
	@Output() combinedOrderCellMouseActionEvent = new  EventEmitter<ICombinedOrderCellMouseActionEvent>();
	@HostBinding('class.cell-highlighted') highlightedClass: boolean;

	constructor() {}

	@HostListener('mouseenter') onMouseEnter() {
		if (this.orderMTUData.combined) {
			this.combinedOrderCellMouseActionEvent.emit({gridGroupKey:  this.gridGroupKey,cellColumnIndex:  this.columnIndex,mouseAction:  'mouseenter'});
			this.highlightedClass = true;
		}
	}

	@HostListener('mouseleave') onMouseLeave() {
		if (this.orderMTUData.combined) {
			this.combinedOrderCellMouseActionEvent.emit({gridGroupKey:  this.gridGroupKey, cellColumnIndex:  			this.columnIndex, mouseAction:  'mouseleave'});
		}
		this.highlightedClass = false;
		}
}
```

html integration
```
<td  [columnIndex]="j"  [gridGroupKey]="ordersRow.groupKey"  [appCombinedOrdersMouseover]="orderMTUData"  (combinedOrderCellMouseActionEvent)="onCombinedOrderCellMouseAction($event)"></td>
```

### Pipes

Used to transform bound properties before display

#### Angular Built-in
* date
* json, slice
* number, decimal, percent, currency
* uppercase lowercase

e.g. `{{ product.price | currency | uppercase }}`

#### Custom
```
@Pipe({name: 'duration'})
export class DurationPipe implements PipeTransform {
	transform(value: number): string {
		const transformedValue = '';
		...
		return transformedValue;
	}
}
```

### Forms

#### Template-driven forms, [docs](https://angular.io/guide/forms)
Utilize form directives ngModel, ngForm, ngSubmit.
Example:
```
<form #loginForm="ngForm" (ngSubmit)="login(loginForm.value)">
	<div class="form-group">
		<em *ngIf="loginForm.controls.userName?.invalid && (loginForm.controls.userName?.touched || mouseoverLogin)">Required</em>
		<input type="text" (ngModel)="userName" class="form-control" required>
	</div>
	<span (mouseenter)="mouseoverLogin=true" (mouseleave)="mouseoverLogin=false">
		<button type="submit" [disabled]="loginForm.invalid">Login</button>
	</span>
</form>
```
#### Reactive forms
Utilize Angular objects FormGroup & FormControl.
Perform field validation with Angular built-in Validators (required, pattern) and custom validation methods

### template

* ng-template
* ng-container


#### Safe Navigation Operator (?.)
[Documentation](https://www.concretepage.com/angular-2/angular-2-pipe-operator-and-safe-navigation-operator-example)



### Binding

#### Interpolation

1-way binding from component class to template 
`<img src='{{product.imageUrl}}'>`

#### Property Binding
bind to DOM properties
`<img [src]='product.imageUrl'>` 

#### Event Binding
`<button (click)="clickButton()">`

#### Class Binding
`<div [class.green]="shouldDivBeGreen()"></div>`

#### 2-way Binding
`<input [(ngModel)]='listFilter'`

### Component Communication

1. @Input properties to pass data from parent to child component
2. @Output properties to pass data from child to parent component
3. template reference using `#`. Example:
```
// in following html we use template reference to invoke component method logFoo()
<event-thumbnail #thumbnail></event-thumnail>
<button (click)="thumbnail.logFoo()"></button>
```


### ViewChild

[Reference](https://angular.io/api/core/ViewChild)
Property decorator that configures a view query. The change detector looks for the first element or the directive matching the selector in the view DOM. If the view DOM changes, and a new child matches the selector, the property is updated.

### View Encapsulation 

[Reference](https://angular.io/guide/view-encapsulation)

In Angular, component CSS styles are encapsulated into the component's view and don't affect the rest of the application. To control how this encapsulation happens on a per component basis, you can *set the view encapsulation mode in the component metadata*.

### Content Projection

 

### OnPush Change Detection Strategy

Utilize in presentational components. With onPush, Change Detection Cycle is triggered when: 
* Input params change (new value). ngOnChanges is triggered
* Component's event handler is triggered
* An observable linked to the template via the async pipe emits a new value 


### Routing

[Angular LocationStrategy](https://angular.io/guide/router#locationstrategy-and-browser-url-styles)
* [PathLocationStrategy](https://angular.io/api/common/PathLocationStrategy) HTML5 pushState style
* [HashLocationStrategy](https://angular.io/api/common/HashLocationStrategy) Url hash style

Use routerLink attribute in tag `<a>` for navigating to a route and template tag`<router-outlet>` as the wrapper of the component loaded by the router.
```
<ul class='nav navbar-nav'>
	<li><a [routerLink]="['/welcome']">Home</a></li>
	<li><a [routerLink]="['/products']">Products List</a></li>
</ul>
<router-outlet></router-outlet>
```

`Routes` sample from ETSS WT
```
const  routes: Routes = [
{
path:  'results',
component:  ResultsComponent,
children: [
	{ path:  'prices/:' + Constants.routeParams.date , component:  PricesTableComponent },
	{ path:  'schedules/:' + Constants.routeParams.date, component:  SchedulesTableComponent },
	{ path:  'penalties/:' + Constants.routeParams.date, component:  PenaltiesTableComponent },
]},];
@NgModule({
	imports: [RouterModule.forChild(routes)],
	exports: [RouterModule]
})
```

#### RoutingGuards

Create service (named for example `RoutingGuard`) that implements interface `CanActivate` and implement method `canActivate` that will return true or false depending on whether the route should be activated for example if user is logged-in. Add also property `canActivate` in route definition.
e.g.
```
{
	path:  'product/:productId',
	canActivate: [RoutingGuard],
	component:  ProductDetailsComponent
},
```

#### pre-loading data

Use property `resolve` in route definition to pre-load data necessary to the corresponding component.

#### lazy loading

Load bundles/modules on demand.
To lazy load module UserModule do the following:
1. create corresponding routes, userRoutes
`export const userRoutes = [{path: 'profile', component: ProfileComponent}];`
2. add following path in appRoutes
`{ path: 'user', loadChildren: './user/user.module#UserModule' }`



### Compilers

#### Just in Time (JIT)
Used for development.
Compiles in the browser at runtime.
Not used by default for Angular v9+

#### Ahead of Time (AOT)
Used for deployment.
Pre-compiles before deploying and thus compiler source code is not deployed along with our app's source code.
As of Angular v9+ AOT compiler is used during development and it pre-compiles before serving.


#### Angular Ivy

[Reference](https://angular.io/guide/ivy#angular-ivy)

### Server-side rendering (SSR) with Angular Universal
[Reference](https://angular.io/guide/universal)

A Node.js Express web server compiles HTML pages with Universal based on client requests.

### ETSS WebTrader

#### Architecture

Module (view) based architecture in ETSS app.
Each view folder contains
* files
	*  *.module.ts
	* *-routing.module.ts
* folders
	* components
	* models
	* pages

#### Utilize OAuth2 protocol for authentication

package `angular-oauth2-oidc`
Retrieve JWT token from IDM and send it to Application Server. Add JWT to http header `authorization`
Set the header using Angular's `HttpInterceptor`.


> Written with [StackEdit](https://stackedit.io/).