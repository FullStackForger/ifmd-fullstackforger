Todo
- keep in touch
  - http://www.yearofmoo.com/
  - http://angularair.com/
  - https://devchat.tv/adventures-in-angular
- angular material
  https://material.angularjs.org/
- angular setup
  - protractor
- Angular Framework
  - when to use angular and when not to https://docs.angularjs.org/guide/introduction
  - declarative (angular) vs imperative (jquery)
    https://en.wikipedia.org/wiki/Declarative_programming
    https://en.wikipedia.org/wiki/Imperative_programming
  - good practices (angular zen from https://docs.angularjs.org/guide/introduction)
- dependency injection, execution order: http://jsfiddle.net/ysq3m/
- digestion cycle ($watch, $apply, $digest)
- controllers (view agnostic)
  - scopes: $rootScope, $scope
  - scope digest cycle $apply -> $digest -> $watch
  - $watch dirty checking strategies
    - by reference (`$scope.$watch(expression, listener)`)
      - detects change if whole value changes
      - most efficient
      - ignores internal changes
    - by collection (`$scope.$watchCollection(expressino, listener)`)
      - detects if items of object or arrays change
      - shallow checking
      - more expensive to perform
    - by value (`$scope.$watch(expression, listener)`)
      - detects any change in nested data structure
      - most powerful
      - most expensive to run
- directives
  - type of directives
    - observing directives register listeners using `$watch`
    - listener directives register DOM listener and update view calling `$apply`
- components
- debugging
  - debug directive   https://egghead.io/lessons/angularjs-build-a-debug-directive
- filters
  - should be stateless and [idempotent](https://en.wikipedia.org/wiki/Idempotence)
- forms
  - $parsers and $formatters
  - ng-messages
  - links
    - [Complex forms with Advanced Directives in AngularJS](https://www.youtube.com/watch?v=G5MzkDJQkoQ)


 - karma testing
  - single run `karma start karma.conf.js  --log-level debug --single-run`
  - isolate test `it()` > `fit()`
  - disable test `it()` > `xit()`
  - tutorials:
    https://www.youtube.com/watch?v=YRzr27Bpx_g&index=1&list=PLw5h0DiJ-9PDbh2i6knU4FybWA63PPbVi
  - mock service
```
beforeEach(function () {
  module(function ($provide) {
    $provide.value('schedule', { /* api */ })
  })
})
```
  - mock filter
Angular stores filter like services but adds `Filter` to the end of the filter service name.
```
var mockFilter = function() {
    return function (input) { return input }
}

beforeEach(function() {
    module(function($provide) {
        $provide.value('dateFilter', mockFilter )
    })
})
```


Protractor
-----------

 - protractor (https://www.youtube.com/watch?v=idb6hOxlyb8)
  - what is protractor
    - angular E2E testing framework    
    - introduced with Angular 1.2 release
    - goal: improved e2e testing    
  - why protractor
    - build on the top of web-driver
        - nodejs wrapper for selenium server
    - better syntax for writing tests
    - allows running tests remotely
      - powerful for testing different environments
    - can run tests in multiple browsers at once (selenium grid)
    - has it's own runner (karma runner not required)
    - works well with both Jasmine and Mocha
  - Getting started (1)
    - installing protractor
      `npm install -g protractor`
    - install web-driver
      `webdriver-manager update`
    - start server with
      `webdriver-manager start`
  - Getting started (2)
    - configuring protractor (per environment)
      - selenium server configuration
      - where are spec files located
      - browser capabilities required per spec (chrome, safari, etc.)
      - base url for spec files
      - jasmine node configuration
  - Getting started (3)
    - configuring selenium server
      - seleniumServer.jar and seleniumPort for standalone
      - selenium url address
      - sauceUser/sauceKey for remote via SauseLabs
    - specs - array of file patterns
      `[specs: 'test/scenerios/**/*.scenerios.js']`
    - base url - base url that will replace relative paths using `get()`
      `baseUrl: 'http://localhost:9000'`
    - jasmineNodeOpts - an array of options for jasmine node output
       - `onComplete` - method called before driver quits
       - `isVerbose` - set to `true` for verbose output
       - `showColors` - colored output
       - `includeStackTrace` - includes stack trace when errors

   - protracotr instance
    `var ptor = protractor.getInstance()`
   - protractor timeout
    Jasmine default timeout is 5s so for E2E tests might be too low.
    Increase it with 3rd param passed to a test
    ```js
    it('should do something',  function () {
      /* test spec */
    }, 20000) // <- will set it to 20s
    ```

  - Protractor locators

    `element(by.x('....'))`
    or
    `elements(by.x('....'))`   <!--- todo: ???? --->     

    - locators are synchronous in nature
    - locators will throw if element can't be found
    - `ptor.findElements()` returns all available elements

 - Available locators
 Protractor
 http://angular.github.io/protractor/#/api?view=ProtractorBy

 ```
`addLocator`	Add a locator to this instance of ProtractorBy.
`binding`	Find an element by text binding.
`exactBinding`	Find an element by exact binding.
`model`	Find an element by ng-model expression.
`buttonText`	Find a button by text.
`partialButtonText`	Find a button by partial text.
`repeater`	Find elements inside an ng-repeat.
`exactRepeater`	Find an element by exact repeater.
`cssContainingText`	Find elements by CSS which contain a certain string.
`options`	Find an element by ng-options expression.
`deepCss`	Find an element by css selector within the Shadow DOM.
```

Inherited by webdriver
http://angular.github.io/protractor/#/api?view=webdriver.By

`className`	Locates elements that have a specific class name.  
`css`	Locates elements using a CSS selector.  
`id`	Locates an element by its ID.  
`linkText`	Locates link elements whose visible text matches the given string.  
`js`	Locates an elements by evaluating a JavaScript expression.  
`name`	Locates elements whose name attribute has the given value.  
`partialLinkText`	Locates link elements whose visible text contains the given substring.  
`tagName`	Locates elements with a given tag name.  
`xpath`	Locates elements matching a XPath selector.  


** Long list of `element` API methods
https://angular.github.io/protractor/#/api?view=ElementFinder

Protractor

`then`	Access the underlying actionResult of ElementFinder.  
`clone`	Create a shallow copy of ElementFinder.  
`locator`	See `ElementArrayFinder.prototype.locator`  
`getWebElement`	Returns the WebElement represented by this ElementFinder.  
`all`	Calls to all may be chained to find an array of elements within a parent.  
`element`	Calls to element may be chained to find elements within a parent.  
`$$`	Calls to $$ may be chained to find an array of elements within a parent.  
`$`	Calls to $ may be chained to find elements within a parent.  
`isPresent`	Determine whether the element is present on the page.  
`isElementPresent`	Same as ElementFinder.isPresent(), except this checks whether the element identified by the subLocator is  present, rather than the current element finder.  
`evaluate`	Evaluates the input as if it were on the scope of the current element.  
`allowAnimations`	See ElementArrayFinder.prototype.allowAnimations.  

Inherited from webdriver

`findElements`	Schedules a command to find all of the descendants of this element that match the given search criteria.  
`click`	Schedules a command to click on this element.  
`sendKeys`	Schedules a command to type a sequence on the DOM element represented by this instance.  
`getTagName`	Schedules a command to query for the tag/node name of this element.  
`getCssValue`	Schedules a command to query for the computed style of the element represented by this instance.  
`getAttribute`	Schedules a command to query for the value of the given attribute of the element.  
`getText`	Get the visible text.
`getSize`	Schedules a command to compute the size of this element's bounding box, in pixels.
`getLocation` Schedules a command to compute the location of this element in page space.  
`isEnabled`	Schedules a command to query whether the DOM element represented by this instance is enabled, as dicted by the disabled attribute.  
`isSelected`	Schedules a command to query whether this element is selected.  
`submit`	Schedules a command to submit the form containing this element (or this element if it is a FORM element).  
`clear`	Schedules a command to clear the value of this element.  


## Shorter list of `element` API methods
Probably most commonly used elements.

 - `clear()` - clears input.  
 - `click()` - triggers click event.  
 - `getAttribute(name)` - gets value of the attribute.  
 - `getCssValue(propertyName)` - gets value of css.  
 - `getLocation()` - position on the page.  
 - `getSize()` - width and height of the element.  
 - `getTagName()` - get the tag name of the element.  
 - `sendKeys()`	- emulates user input.  
 - `isSelected()` - checks if element is selected.  
 - `isEnabled()` - checks if element is selected.  
 - `isDisplayed()`- checks if element is currently displayed.  
 - `getInnerHtml()`	- Schedules a command to retrieve the inner HTML of this element.
 - `takeScreenshot()`- Take a screenshot of the visible region restricted by element rectangle.

 ### Additional action sequences possibilities
  - multikey scenerios
    - keyboard combos
    - mouse clicks + keyboard clicks
  - drag and drop

## Global actions

  `browser.waitForAngular()` - Instruct webdriver to wait until Angular has finished rendering and has no outstanding $http or $timeout calls before continuing. Note that Protractor automatically applies this command before every WebDriver action.
  `browser.get(url)` - navigates to URL (assumes you are testing Angular app)
  `browser.getCurrentUrl()` - returns current url
  `element(by.X)`
  `element.all()`

## Protractor Syntax vs WebDriverJS Syntax


|WebDriver Syntax |	Protractor Syntax|
|-----------------|------------------|
| webdriver.By |	by |
| browser.findElement(...) | element(...) |
| browser.findElements(...)	| element.all(...) |
| browser.findElement(webdriver.By.css(...)) | $(...) |
| browser.findElements(webdriver.By.css(...))	| $$(...) |


## Writing tests

### Missed scenerios

https://egghead.io/lessons/angularjs-use-protractor-to-catch-errors-in-the-console
Checking console output for errors
