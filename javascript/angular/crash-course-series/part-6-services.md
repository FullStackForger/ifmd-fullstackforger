# Angular Crash Course - Part 6<br/>Services


## Service in software engineering

Self-contained building blocks of application functionality.

## Angular services

Angular application building blocks wired together using dependency injection (DI) provided by `$injector` service.

- Build-in services start with $, eg: $https
- Used to organise code
- Lazily instantiated
- Singletons
- Can be injected (DI)

### `$injector` service

The injector service instantiate only two types of objects, **custom services** and **specialized objects**.  

**Specialized objects** are part of Angular API.   
**Services** are objects with API designed by developer.  

### Recipes

Helper methods for registering components with `$injector`

### Specialized object recipes

- controllers
- directives
- filters
- animations

### Service recipes (providers)

- value
- constant
- factory
- service
- provider

Provider recipe is the most verbose and the most comprehensive one. All other recipes (Value, Factory, Service and Constant) are syntactic sugar created on top of a provider recipe.

### Angular service example

```
angular
  .module('myServiceModule', [])
  .factory('queue', queueFactory)
  .controller('MyController', MyController)

function() queueFactory {
  return {
    add: function (obj) { /* ... */ }
  }
}

function MyController($scope, queue) {
    queue.add('item')
}  
```
## Dependency Injection (DI)

When **service** is requested, **`$injector`** service finds correct **service provider**, instantiate it and calls its **`$get()` service factory function** to get the **instance of the service**.

### Terminology

**$injector** service is used to retrieve object instances as
defined by provider, instantiate types, invoke methods, and
load modules.

**Service providers** are constructor functions. When
instantiated they must contain a property called `$get`,
which holds the **service factory** function.

**Service factories** are functions created by a service
provider and used to create service objects.

**Service** is a singleton object created by a service factory.

### DI Diagram
[angularjs-concept-diagram](http://stepansuvorov.com/blog/2015/04/angularjs-concept-diagram/)

## Service recipes

### Value recipe

- Value service does what it says on the box
- Returns primitive type value ( string, number, boolean)
- Returns composite type value ( object, array )
- Makes value available for dependency injection

```
angular
  .module("meanApp", [])
  .value("user", {
    token: "SUPER-SECRET-TOKEN",
    name: "MEAN User"
})
```

### Constant recipe

- Available in run phase ( same as value recipe )
- Available in configuration phase ( same as provider recipe)

```
myApp.constant('uriPrefix', ‘user-')

meanApp.config(function(userProvider, uriPrefix) {
  userProvider.setUriPrefix(uriPrefix)
})
```

### Factory recipe

- Most common, and used in majority of situations
- If you like modular pattern
- Explicit object construction allows “private” properties
- Returns object with public API methods

```
angular.module("meanApp", [])
  .factory("user", function() {
    var userName = "Mean User"
    return {
      token: "SUPER-SECRET-TOKEN",
      name: userName
    }
  })
```

Can be used to instantiate external libraries.

```
function User(userId) {
 this.token = userId
 this.name = "Mean User"
}

angular.module("meanApp", [])
 .value("userId", 123)
 .factory("user", function(userId) {
   return new User(userId)
})
```

### Service recipe

- Instantiating external libraries
- When you like using keyword this over modular pattern
- Better for object constructor methods

```
function User(userId) {
  this.token = userId;
  this.name = "Mean User";
}

angular.module("meanApp", [])
  .value("userId", 123)
  .service("user", ["userId", User])
```

### Provider recipe

- Available in configuration phase
- Configurable reusability
- Great for instantiating external configurable libraries

```
var meanApp = angular.module("meanApp", [])

meanApp.provider('user', function UserProvider() {
  var uriPrefix = ""

  this.setUriPrefix = function(uri) {
    uriPrefix = uri
  };

  this.$get = ["userId", function userFactory(userId) {
    return new User(userId, uriPrefix);
  }]
})

meanApp.config(function(userProvider) {
  userProvider.setUriPrefix("user-")
})
```

## Service comparison

Source: [angularjs/guide/providers](https://docs.angularjs.org/guide/providers)

Features / Recipe type | Factory	| Service	| Value	| Constant	| Provider
-------|-------|-------|-------|-------|-------
can have dependencies	| yes	|	yes	|	no	|	no	|	yes
uses type friendly injection	|	no	|	yes	|	yes*	|	yes*	|	no
object available in config phase	|	no	|	no	|	no	|	yes	|	yes**
can create functions	|	yes	|	yes	|	yes	|	yes	|	yes
can create primitives	|	yes	|	no	|	yes	|	yes	|	yes

\* at the cost of eager initialization by using new operator directly  
\*\* the service object is not available during the config phase, but the provider instance is
