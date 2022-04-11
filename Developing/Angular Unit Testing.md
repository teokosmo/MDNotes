# Unit Testing

## fakeAsync, tick & spyOn to simulate data retrieval from services

**Examples**

1. [Codehandbook](https://codehandbook.org/how-to-unit-test-angular-component-with-service/)
2. [StackOverflow](https://stackoverflow.com/a/53344611)


## Mock services (test doubles)

**References**
1. <https://angular.io/guide/testing-components-scenarios#provide-service-test-doubles>
2. <https://testing-angular.com/testing-components-depending-on-services/>
3. <https://infinum.com/handbook/frontend/angular/angular-guidelines-and-best-practices/testing#typing-test-doubles>

### Basic requiremenets

1. Equivalence of fake and original: The fake must have a type derived from the original.
2. Effective faking: the original stays untouched.

[Reference](https://testing-angular.com/faking-dependencies/#faking-dependencies)

## Get service instance inside test script

1. In beforeEach hook with TestBed.inject() e.g. `nameOfService = TestBed.inject(NameOfService);` 
    
    [Reference](https://angular.io/guide/testing-components-scenarios#testbedinject)
2. In it() with fixture’s injector.get() e.g. `const nameOfService = fixture.debugElement.injector.get(NameOfService);`

    [Reference](https://angular.io/guide/testing-components-scenarios#get-injected-services)


## Override component providers

When a component injects a service in its providers `@Component({ …, providers: […] }),`

then stubbing the service in the providers of TestBed.configureTestingModule is not possible. Also it is not possible to get the instance of this service via fixture’s injector.

To overcome this utilize TestBed.overrideComponent

[Reference](https://angular.io/guide/testing-components-scenarios#override-component-providers)

## Angular Testing Utility APIs

[Reference](https://angular.io/guide/testing-utility-apis)

## Testing HTTP requests

**References**

1. <https://angular.io/guide/http#testing-http-requests>
2. <https://testing-angular.com/testing-services/#testing-a-service-that-sends-http-requests>


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