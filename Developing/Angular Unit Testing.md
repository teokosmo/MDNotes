# Unit Testing

## fakeAsync, tick & spyOn to simulate data retrieval from services

**Examples**

1. [Codehandbook](https://codehandbook.org/how-to-unit-test-angular-component-with-service/)
2. [StackOverflow](https://stackoverflow.com/a/53344611)


## Mock services (test doubles/stubs)

**References**
1. <https://angular.io/guide/testing-components-scenarios#provide-service-test-doubles>
2. <https://testing-angular.com/testing-components-depending-on-services/>
3. <https://infinum.com/handbook/frontend/angular/angular-guidelines-and-best-practices/testing#typing-test-doubles>
4. <https://angular.io/guide/testing-components-scenarios#override-component-providers>, in the context of overriding component service providers

### Faking dependencies requirements

1. Equivalence of fake and original: The fake must have a type derived from the original.
2. Effective faking: the original stays untouched.

[Reference](https://testing-angular.com/faking-dependencies/#faking-dependencies)

## Get service instance inside test script

1. In beforeEach hook with TestBed.inject() e.g. `nameOfService = TestBed.inject(NameOfService);` 
    
    [Reference](https://angular.io/guide/testing-components-scenarios#testbedinject)
2. In it() with fixture’s injector.get() e.g. `const nameOfService = fixture.debugElement.injector.get(NameOfService);`

    [Reference](https://angular.io/guide/testing-components-scenarios#get-injected-services)

## Mock service properties

Use `Object.defineProperty`, for example to mock `JointOfferService.devices` do the following:
```
Object.defineProperty(jointOfferService, 'devices', { value: [tmpDevice] });
```
[Reference](https://stackoverflow.com/a/69798524)

## Override component providers

When a component injects a service in its providers `@Component({ …, providers: […] }),`

then stubbing the service in the providers of TestBed.configureTestingModule is not possible. Also it is not possible to get the instance of this service via fixture’s injector.

To overcome this utilize `TestBed.overrideComponent`

**References**

1. <https://angular.io/guide/testing-components-scenarios#override-component-providers>
2. <https://medium.com/ngconf/how-to-override-component-providers-in-angular-unit-tests-b73b47b582e3>

## Testing Attribute Directives

<https://angular.io/guide/testing-attribute-directives>

## Angular Testing Utility APIs

[Reference](https://angular.io/guide/testing-utility-apis)

## Testing HTTP requests

**References**

1. <https://angular.io/guide/http#testing-http-requests>
2. <https://testing-angular.com/testing-services/#testing-a-service-that-sends-http-requests>


## Routing testing

[Reference](https://dev.to/this-is-angular/testing-angular-routing-components-with-the-routertestingmodule-4cj0)

## Data attibutes for testing purposes

use attribute `data-testid` for selecting DOM elements in tests

**References**

1. <https://github.com/vtex/styleguide/issues/848>
2. <https://medium.com/agilix/angular-and-cypress-data-cy-attributes-d698c01df062>
3. <https://docs.cypress.io/guides/references/best-practices>


## Automatic change detection

Use service `ComponentFixtureAutoDetect` in testing module providers like so
```
{ provide: ComponentFixtureAutoDetect, useValue: true }
```
The `ComponentFixtureAutoDetect` service responds to asynchronous activities such as promise resolution, timers, and DOM events. But a direct, synchronous update of the component property is invisible. The test must call fixture.detectChanges() manually to trigger another cycle of change detection.

[Reference](https://angular.io/guide/testing-components-scenarios#automatic-change-detection)

## Nested components

Create stub components to emulate nested components
e.g.
```
@Component({selector: '[px-basket]', template: ''})
  class BasketStubComponent {
}
```

[Reference](https://angular.io/guide/testing-components-scenarios#nested-component-tests)

## Component lifecycle hooks

For each spec (it) TestBed method `createComponent`, usually contained in beforeEach, is executed.

`CreateComponent` executes component's following methods in order of appearance
1. constructor
2. ngOnInit hook

**References**

1. <https://www.testim.io/blog/angular-component-testing-detailed-guide/>
2. <https://stackoverflow.com/a/49234888>
3. <https://www.damirscorner.com/blog/posts/20210101-TestingAngularLifecycleHooks.html>

## spyOn window.location object

1. inject window.location in component as described in <https://itnext.io/testing-browser-window-location-in-angular-application-e4e8388508ff> and then mock injection token as described in <https://www.reddit.com/r/angular/comments/p35yz2/mock_locationreload_in_jasminekarma_test_angular/h8s6y56/?utm_source=share&utm_medium=web2x&context=3>

i.e. 
```
// ****************
// app.component.ts
// ****************
export const LOCATION_TOKEN = new InjectionToken<Location>('Window location object');
@Component({
  providers: [
    { provide: LOCATION_TOKEN, useValue: window.location }
  ]
})
export class AppComponent {
  constructor(@Inject(LOCATION_TOKEN) private location: Location) {}
  
// *********************
// app.component.spec.ts
// *********************
const locationStub = {
  search: 'proximus.be/?v5=starter',
  assign: jasmine.createSpy().and.callFake(() => {}),
};
beforeEach(async () => {
        await TestBed.configureTestingModule({
            declarations: [],
            imports: [

            ],
            providers: []
        })
            .overrideComponent(PackSwitcherComponent, {
              set: {
                providers: [{ provide: LOCATION_TOKEN, useValue: locationStub }],
              },
            })
            .compileComponents();
    });
```

2. spyOn component method that contains window reference as described in
<https://thetombomb.com/posts/stubbing-location-reload-in-jasmine-tests>