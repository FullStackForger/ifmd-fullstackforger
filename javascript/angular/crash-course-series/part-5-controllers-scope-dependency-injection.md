# Angular Crash Course - Part 5<br/>Controllers, scope, dependency injection

## What is Controller?

[Controllers](https://docs.angularjs.org/guide/controller) are JavaScript constructor methods used to augment the Angular Scope.
They attach to DOM via the `ng-controller` directive.

Controller has two major responsibilities:
- Set up the initial state of the $scope object &#10004;
- Add behavior to the $scope object &#10004;

**Don't use controller to:**

- Manipulate DOM	(databindings and directives instead) &#10008;
- Format input  (form controls) &#10008;
- Filter output	(filters) &#10008;
- Share code or state (services) &#10008;
- Manage life-cycle (angular services auto-instantiate) &#10008;

> In principle Controller **SHOULDN’T** try to do too much.  
> **SHOULD** encapsulate business logic for a single view.

### Simple controller example

```
<script>
var myApp = angular.module('myApp',[]);

myApp.controller('GreetingController', ['$scope', function($scope) {
  $scope.greeting = 'Hola!';
}]);
</script>

<div ng-controller="GreetingController">
  {{ greeting }}
</div>

<div ng-controller="GreetingController">
  {{ greeting }}
</div>
```

### Controller As

```
<script>
var myApp = angular.module('myApp',[]);

myApp.controller('GreetingController', ['$scope', function($scope) {
  this.greeting = 'Hola!';
}]);
</script>

<div ng-controller="GreetingController as ctrl">
  {{ ctrl.greeting }}
</div>
```

Check out Todd Motto's blog post [Digging into Angular’s “Controller as” syntax](https://toddmotto.com/digging-into-angulars-controller-as-syntax/) for more in-depth information on using `controller as` syntax.

## Scope

### JavaScript

#### Execution context

<!--
Sources:  
http://stackoverflow.com/a/9384894
http://www.w3schools.com/js/js_scope.asp
http://davidshariff.com/blog/what-is-the-execution-context-in-javascript/
http://dmitrysoshnikov.com/ecmascript/chapter-1-execution-contexts/
-->

Execution context is an "environment" function executes in.

That includes **variable scope** (and the scope chain, variables in closures from outer scopes), **function arguments**, and the value of the **`this` object**.


#### Call stack

The call stack is a collection of execution contexts.

#### Scope

Scope in JavaScript is set of variables you have access to.

```
var x = 'defined on global scope'

function f() {
    var y = 'defined on local scope'
}
```
`x` can be accessed from anywhere. Once function `f` is invoked, `x` will be in the scope chain (outer scope);

`y` can only be accessed from the level of the code inside `f`() because it is limited to `f`'s scope.
Using `var` keyword restricts a variable to the local scope. Omitting `var` create variable in the global scope. It is commonly called `leaking` and considered a bad practice.

#### Example

```
function Constructor () {
  this.setProp = function ( value ) {
    this.prop = value
  }
}

var prop = 'global'
var obj = new Constructor()

obj.setProp('local')

console.log(prop)           // 'global'
console.log(this.prop)      // 'global'
console.log(obj.prop)       // 'local'

```

Check out [JavaScipt scope and context](http://ryanmorr.com/understanding-scope-and-context-in-javascript/). It is great article explaining how scope and execution context work in JavaScript.

### Angular $scope

Scope is an object that refers to the application model. It is an execution context for expressions.

Scopes are arranged in hierarchical structure which mimic the DOM structure of the application.

Scopes can watch expressions and propagate events.

Read [Understanding Angular Scopes](https://github.com/angular/angular.js/wiki/Understanding-Scopes) to understand better on how Angular scope works in context of JavaScript scopes.

## Dependency Injection (DI)

Dependency Injection (DI) is a software design pattern that describes with how components are provided with dependencies.

Angular handles dependency injection creating components, resolving their dependencies, and providing them to other components as requested.

```
angular
  .module('myApp', [])
  .controller('MyCtrl', ['$scope', '$rootScope', MyCtrl]);

function MyCtrl($scope, $rootScope) ] {
  // controller definition
}
```

```
angular
  .module('myApp', [])
  .controller('MyCtrl', MyCtrl);

MyCtrl.$inject = ['$scope', '$rootScope']

function MyCtrl($scope, $rootScope) ] {
  // controller definition
}
```
