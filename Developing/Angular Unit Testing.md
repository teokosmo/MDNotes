# Unit Testing

## fakeAsync & spyOn to simulate data retrieval from services

[Reference](https://codehandbook.org/how-to-unit-test-angular-component-with-service/)


## Mock services (test doubles)

**References**
1. <https://angular.io/guide/testing-components-scenarios#provide-service-test-doubles>
2. <https://testing-angular.com/testing-components-depending-on-services/>
3. <https://infinum.com/handbook/frontend/angular/angular-guidelines-and-best-practices/testing#typing-test-doubles>

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
[Reference](https://angular.io/guide/http#testing-http-requests)
