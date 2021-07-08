<!-- 

-->
<a name="top"></a>
# International Javascript Conference October 2019 | Munich

- [International Javascript Conference October 2019 | Munich](#international-javascript-conference-october-2019--munich)
  - [Ivy, Angular compiler](#ivy-angular-compiler)
  - [Web Components with Angular Elements](#web-components-with-angular-elements)
  - [Creating Custom Structural Directives](#creating-custom-structural-directives)
    - [Directives](#directives)
    - [Views & Dynamic views](#views--dynamic-views)
    - [Steps to create structural directives](#steps-to-create-structural-directives)
    - [Magic behind the asterisk](#magic-behind-the-asterisk)
  - [Are you being servered? Exploring a serverless Web](#are-you-being-servered-exploring-a-serverless-web)
    - [PRE-RENDERING](#pre-rendering)
      - [Jamstack Advantages](#jamstack-advantages)
        - [SECURITY](#security)
        - [PERFORMANCE](#performance)
        - [SCALE](#scale)
    - [SERVERLESS RENDERING](#serverless-rendering)
    - [Resources](#resources)
  - [Profiling Javascript apps like a pro](#profiling-javascript-apps-like-a-pro)
    - [Understanding Browsers](#understanding-browsers)
    - [RAIL Model](#rail-model)
    - [Profiling Process](#profiling-process)
    - [Memory Leaks](#memory-leaks)
    - [Angular Performance Tuning Tips](#angular-performance-tuning-tips)

## Ivy, Angular compiler

## Web Components with Angular Elements

<div style="page-break-after: always; visibility: hidden"> 
\pagebreak 
</div>

## Creating Custom Structural Directives

> Ashnita Bali | [Twitter](https://twitter.com/ashnita01)

### Directives

Directives are HTML elements and attributes created by Angular applications and Angular libraries.

 - Components
   - View (html)
   - Styles (css)
   - Data & methods (ts)
 - Attribute Directives (Appearance and behaviour)
    Changes host element's appearance or behaviour
 - Structural Directives (Views structure)
    Changes view structure

**Examples**

![Screenshot from 2019-12-03 00-03-37](https://i.imgur.com/bxCGXyI.png)


### Views & Dynamic views

A view is the smallest grouping of display elements that can be ```created``` and ```destroyed``` together.

![Screenshot from 2019-12-03 00-07-06](https://i.imgur.com/xLjdudW.png)

Structural directives implement the logic to render embedded views.

![Screenshot from 2019-12-03 00-08-39](https://i.imgur.com/g2r8QKd.png)


Create structural directives when
- rendering a view based on some reusable logic:
  *appHasRole, *appHasPermission, *appHasFeature
- only implement the functionality, allowing the user to define the view:
  *appCarousel
- declare variales
  *appLet

### Steps to create structural directives

**Plan**
- What logic does the structural directive implement?
- API: input properties, context object

```html
<div *appHasRole="'admin'">
  User with admin role can see this
</div>
```

1. `ng g d hasRole`
```typescript
  import {Directive} from '@angular/core';
  @Directive({
    selector: '[appHasRole]'
  })
  export class HasRoleDirective {
    constructor() {}
  }
```
2. Get the TemplateRef instance
```typescript
  export class HasRoleDirective {
    constructor(
      private templateRef: TemplateRef<HasRoleContext>
    ) {}
  }
```
```html
<ng-template [appHasRole]="'admin'">
  <div>User with admin role can see this</div>
</ng-template>
```
3.Get the ViewContainerRef instance
```typescript
  export class HasRoleDirective {
    constructor(
      private vcr: ViewContainerRef,
      private templateRef: TemplateRef<HasRoleContext>
    ) {}
  }
```

![Screenshot from 2019-12-03 00-41-02](https://i.imgur.com/xXpIAcS.png)

![Screenshot from 2019-12-03 00-43-20](https://i.imgur.com/i7c6moN.png)

4.Input properties
```typescript
  @Directive({
    selector: '[appHasRole]'
  })
  export class HasRoleDirective {
    @Input() appHasRole;
    @Input() appHasRoleElse: TemplateRef<HasRoleContext>;
    @Input() appHasRoleThen: TemplateRef<HasRoleContext>;
  }
```
Input property name = Directive attribute name
```html
<ng-template [appHasRole]="'admin'">
  <div>User with admin role can see this</div>
</ng-template>
```
```html
<div *appHasRole="'customer'";else elseRef>
  <div>User with customer role can see this</div>
</div>
<ng-template #elseRef>Render if user doesn't have role</ng-template>
```

5.Context object

Passing context to embedded view instance

`vcr.createEmbeddedView(templateRef, context);`

Context object
```typescript
export class HasRoleDirective {
  @Input() appHasRole;
  context = {
    $implicit: this.appHasRole,
    role: this.appHasRole
  }
}
```

```typescript
export class CarouselDirective {
  @Input() appCarouselForm;
  index = 0;
  context = {
    $implicit: this.appCarouselForm[index],
    control: {
      next: () => this.next(),
      previous: () => this.previous()
    }
  }
}
```

Template input variables access context properties

![Screenshot from 2019-12-03 00-56-16](https://i.imgur.com/wdpXYRw.png)

ngFor

```typescript
export class NgForOfContext<T> {
  constructor(
    public $implicit: T, public ngForOf: NgIterable<T>, public index: number, public count: number
  ){}
  get first(): boolean { return this.index === 0;}
  get last(): boolean { return this.index === this.count -1;}
  get even(): boolean { return this.index % 2 === 0;}
  get odd(): boolean { return !this.even;}
}
```

6.Implement logic
```typescript
export class HasRoleDirective {
  @Input() appHasRole;
  constructor(private roleService: RoleService) {}
  ngOnInit() {
    this.sub = this.roleService.user$
        .pipe(filter(roles => rolse.includes(this.appHasRole)))
        .subscribe(() => {
          this.vcr.clear();
          this.vcr.createEmbeddedView(this.templateRef)
        })
  }
}
```

### Magic behind the asterisk



<div style="page-break-after: always; visibility: hidden"> 
\pagebreak 
</div>

## Are you being servered? Exploring a serverless Web

> [Phil Hawksworth](https://www.hawksworx.com/) | [Twitter](https://twitter.com/philhawksworth)

- Functions as a service
- Serving websites without webservers
- jamstack, Javascript / API / Markup

**jamstack** <br/>A modern architecture.
Create fast and secure sites and dynamic apps with Javascript, APIs, and pre-rendered Markup, <u>served without web servers</u>.

**static sites**
- https://reactjs.org/
- https://yarnpkg.com/lang/en/
- https://vuejs.org/
- https://justdoit.nike.com/

**a little more "dynamic" sites**

- https://www.smashingmagazine.com/
- https://squoosh.app/
- https://www.hawksworx.com/

**illustrating points**

- very low deployment friction
- events and automation

**2 principles**

- pre-rendering
- serverless rendering


### PRE-RENDERING

- doing the work now, so your servers don't have to later
- put distance between the complexity and the user

![Screenshot from 2019-11-14 00-15-45](https://i.imgur.com/ZeWyJUh.png)


#### Jamstack Advantages

![Screenshot from 2019-11-14 00-16-41](https://i.imgur.com/4blfU5I.png)

##### SECURITY

- a greatly reduced surface area
- far fewer moving parts to attack


##### PERFORMANCE

Traditional stacks add static layers in order to improve performance. 
Caching galore

![Screenshot from 2019-11-14 00-19-56](https://i.imgur.com/DQwaDeR.png)

##### SCALE

Traditional stacks add infrastructure in order to scale

![Screenshot from 2019-11-14 00-21-54](https://i.imgur.com/LQzbJ5Q.png)

### SERVERLESS RENDERING

free from infrastructure but not free from dynamic logic

- augmentation
- enhancement
- static first

example: <https://vlolly.net/lolly/3hmtt2r3b/>

**PRE-GENERATED PAGES WITH REAL URLS**
<small>(EVEN FOR THE USER GENERATED CONTENT)<br/>Tool: <u>ELEVENTY</u></small>

**DATA STORED IN A DATABASE**
<small>(BUT DON'T MAKE ME A DBA)<br/>Tool: <u>FAUNADB</u></small>

**INSTANT ACCESS TO NEW CONTENT**
<small>(WITHOUT WAITING FOR A REBUILD)<br/>Tool: <u>NETLIFY FUNCTIONS</u></small>

![Screenshot from 2019-11-25 00-39-24](https://i.imgur.com/L06iVHP.png)

![Screenshot from 2019-11-25 00-41-10](https://i.imgur.com/0NxTd0C.png)


more at <https://css-tricks.com/static-first-pre-generated-jamstack-sites-with-serverless-rendering-as-a-fallback/> 

**enablers**

- custom 404 routing
- events, triggers and automation
- database as a service
- functions as a service

**From** ```seeking to optimize by adding static layers```
**To** ```static first enhancing if required```

![Screenshot from 2019-11-25 00-49-37](https://i.imgur.com/rdntKrX.png)

Simplifying is not dumbing down
It let us focus on what really is important
Easier to reason about

### Resources

- Static site generators, <https://www.staticgen.com/>
- Netlify, <https://www.netlify.com/>
- various related resources from chris coyier, <https://serverless.css-tricks.com/>
- citrix presentation from jamstackconf nyc, <https://www.youtube.com/watch?v=kvS5h5domf0>
- Jamstack conference, <https://jamstackconf.com/>
- headless / decoupled cms, <https://headlesscms.org/>
- About jamstack, <https://jamstack.org/>
- building a url shortener with netlify redirects, <https://www.hawksworx.com/blog/find-that-at/>
- so i guess we're full stack now?, <https://www.youtube.com/watch?v=lFOfQsi5ye0>
- modern web development on the jamstack, <https://www.netlify.com/oreilly-jamstack/>
- jamstack comments example, <https://jamstack-comments.netlify.com/>
- jamstack slack, <https://jamstack.slack.com>

<div style="page-break-after: always; visibility: hidden"> 
\pagebreak 
</div>

## Profiling Javascript apps like a pro

> Gil Fink | [Twitter](https://twitter.com/gilfink)

### Understanding Browsers

**Refresh Rates**

- Devices refresh their screens 60 times a second = 60 fps.
- That means that each frame should take 16ms since 1 second / 60 = 16.66 ms.
- In reality a frame takes ~ 10 ms due to browsers management overhead.

**Pixel pipeline**

![Screenshot from 2019-11-11 00-05-24](https://i.imgur.com/lN2uUSa.png)

- **JavaScript** <br/> used to handle work that will result in visual changes. CSS Animations, Transitions, and the Web Animations API are also
used here.

- **Style** <br/> browser figures out which CSS rules should be applied to elements based on CSS selectors. The style is then calculated to each element.

- **Layout** <br/> the browser calculates how much space each element takes up and where it is on screen. Each element affects other elements in the layout, Web layout model.

- **Paint** <br/> the browser paints all the pixels on screen. It draws every visual part of an element (text, color, images and etc)

- **Composite** <br/> the browser draws the elements according to their layer, if elements overlap each other. Happens on the machine GPU, therefore this step is very fast.


**Reflows**

Reflow might occurs whenever a visual change requires a change in the layout of the page. Examples: browser resize, DOM manipulation and etc. All the flow of the process (pixel pipeline) will run again

**Repaints**

Repaint occurs when a visual change doesn't require recalculation of the whole layout. Examples: element visibility change, changes in text color or background colors and etc. All the flow of the process (pixel pipeline) except *Layout* will run again.

![Screenshot from 2019-11-11 00-27-03](https://i.imgur.com/fRHjXVm.png)

> ![Screenshot from 2019-11-11 00-33-04](https://i.imgur.com/KzprEDa.png)


**Changes without Reflow or Repaint**

JavaScript or CSS changes that don’t affect neither layout or paint. The flow of the process (pixel pipeline) will run again without layout and paint.

![Screenshot from 2019-11-11 00-29-23](https://i.imgur.com/fsyVkBH.png)


### RAIL Model

User-centric performance model
- **Response** <br/> process events in under 50 ms
- **Animation** <br/> produce a frame in 10 ms
- **Idle** <br/> maximize idle time
- **Load** <br/> deliver content and become interactive in under 5 secs

![Screenshot from 2019-11-11 00-39-11](https://i.imgur.com/ijCEdWZ.png)

### Profiling Process

![Screenshot from 2019-11-11 00-40-25](https://i.imgur.com/CgcrJFM.png)


### Memory Leaks

Memory that isn't required by an app but isn't returned to the pool of free memory

**Type of Common Javascript Leaks**

- Accidental global variables <br/> easily solved with *use strict* or using linters
- Forgotten timers or callbacks
- Out of DOM references
- Closures

### Angular Performance Tuning Tips

- OnPush Change Detection
- Lazy Loading Modules
- Preserve Whitespaces
- Enable Production Mode
- ngFor Track By Function
- Avoid Function Calls in Views/Avoid Getters in Views
- Use Pure Pipes <br/> if the input always produce the same output the pipe can be marked as pure
- Use Async Pipes <br/> async pipes automatically handle all observable cleanup