# Angular Crash Course - Part 7<br/>Modules
<!--
source:  
https://speakerdeck.com/rusticode/good-application-development-is-all-about-making-educated-choices-part-2-amd
-->

## Module Definition

> Module is a detachable self-contained unit of a spacecraft  
> source: [Google Search](https://www.google.co.uk/search?q=module+definition)

<p></p>

> Module is a self-contained unit or item, such as an assembly of electronic components and associated wiring or a segment of computer software, which itself performs a defined task and can be linked with other such units to form a larger system.  
> Source: [Collins Dictionary](http://www.collinsdictionary.com/dictionary/english/module)

## Modular encapsulation


```
(high level encapsulation)

                +---------------+
                |  application  |
                +-------+-------+
                        |
             +----------+-------+------------------+
             |                  |                  |
      +------+-------+   +------+-------+   +------+-------+
      |  module M-1  |   |  module M-2  |   |  module M-2  |
      +------+-------+   +--------------+   +--------------+
             |
        +----+-----------------+
        |                      |  
+-------+--------+    +--------+-------+
|  class/obj O-1 |    |  class/obj O-2 |     
+----------------+    +----------------+
|  function A    |    |   function D   |
|  function B    |    |   function E   |
|  function C    |    +----------------+
+----------------+   

(low level encapsulation)
```

## Modular Pattern

> In software engineering, the module pattern is a design
pattern used to implement the concept of software modules,
defined by modular programming, in a programming language
with incomplete direct support for the concept.  
> Source: [Wikipedia](https://en.wikipedia.org/wiki/Module_pattern)

**JavaScript implementation of mudlar pattern**
```js
const myModule = (function () {
  const privateKey = '!DE12d!2e3D'

  function getKey() {
    return privateKey
  }

  return {
    getKey: getKey
  }
})()
```

## Angular module

Module is a **container that allows to group different parts of the application** - controllers, services, filters, directives, etc.

### Defining a module

Modules can be registered with `module()` API method exposed on `angular` object.

Defining main application module would look like below.
```js
angular
  .module('myApp', [])
```

Once defined it can be used to bootstrap angular app.
```html
<!doctype html>
<html lang="en" ng-app="myApp">
<body>...</body>
</html>
```

### Defining a module with dependencies

Modules can load other modules, which allows to better organize web application.

```js
angular
  .module('myApp', [
    'myApp.module1',
    'myApp.module2',
  ])
```

### Module registration API



#### `controller()` method

`controller()` method registers controller constructor function.

```js
angular
  .module('myApp')
  .controller('MyController', MyController)
```

#### `factory()` method

`factory()` method registers service with factory recipe.

```js
angular
  .module('myApp')
  .factory('myFactory', myFactory)
```

#### `service()` method

`service()` method registers service with service recipe.

```js
angular
  .module('myApp')
  .service('myService', myService)
```

#### `filter()` method

`filter()` method registers filter helper.

```js
angular
  .module('myApp')
  .filter('convert', convert)
```

#### `directive()` method

`directive()` method registers directive instruction.

```js
// Adding a directive called "multiDisplay"
angular
  .module('myApp')
  .directive('myDirective', myDirective)
```

### Loading dependencies

#### Configuration block

Configuration method uses provider injector to deliver **Injectable providers**.
Provider configuration is available only from the `config()` method context.
You can call `config()` method more than once per module.

```js
angular
  .module('myModule')
  .config(function(/* injectable providers */) {
    // provider configuration
  })

```

#### Run block

`run()` method uses instance injector to deliver injectable **instances** into a run block, making them available from the level of `run()` method execution context.

```js
.run(function(/*injectables instances */) { // instance-injector
  // run block
});
```

> Run block is the code executed to kickstart the application. It is executed once after all of the services have been configured and the injector has been created.
> Run blocks typically contain code which is hard to unit-test, and for this reason should be declared in isolated modules, so that they can be ignored in the unit-tests.
> source: Angular docs > Module > Module Laoding > [Run Blocks](https://docs.angularjs.org/guide/module)
