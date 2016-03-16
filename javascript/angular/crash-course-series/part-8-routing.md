# Angular Crash Course - Part 8<br/>Routing

## Angular routing

Routing allows to create web application with multiple views and can be enabled by loading Angular module called `ngRoute`.

You can install it with bower.
```bash
bower install angular-route --save
```

Once module is available it has to be included in `index.html` file.
```html
<script src="bower_components/angular-route/angular-route.min.js"></script>
```

### Route configuration

#### Inline templates with `template` option

```js
angular
	.module('myApp', [])
	.config(configure)

function configure($routeProvider) {
	$routeProvider
		.when('/', {
			controller: 'HomePageCtrl',
			template: '<p>{{text}}</p>'
		})
}
```

#### Controller As syntax

```js
angular
	.module('myApp', [])
	.config(configure)

function configure($routeProvider) {
	$routeProvider
		.when('/', {
			controller: 'HomePageCtrl',
			controllerAs: 'homepage',
			template: '<p>{{homepage.text}}</p>'
		})
}
```

#### Auto redirection

```js
angular
	.module('myApp', [])
	.config(configure)

function configure($routeProvider) {
	$routeProvider
		.when('/', {
			controller: 'HomePageCtrl',
			controllerAs: 'homepage',
			template: '<p>{{homepage.text}}</p>'
		})
		.otherwise({
			redirectTo: '/'
		})
}
```

#### Route with external template

Angular allows storing templates as in-line html snippets or as a separate files.
That means you could create a file `app/template/homepage.tpl.html` with following content:
```
<p>{{homepage.text}}</p>
```

Then, use Angular router `templateUrl` option to point controller that external template.

```js
function configure($routeProvider) {
	$routeProvider
		.when('/newsfeed', {
			controller: 'HomePageCtrl',
			controllerAs: 'homepage',
			templateUrl: 'app/template/homepage.tpl.html'
		})
		.otherwise({
			redirectTo: '/'
		});
}
```

#### Route with route parameters

Configuring route with parameters can be done with path.

##### Parsing URI hash

`:param` - exact match, parameter `param` will hold value of matching URI element.

Pattern: `/actor/:actorName`  
Route: `/actor/harrison-ford`  
Match: `actorName: 'harrison-ford'`  

`:param` - wild cart, parameter `param` will hold value of all outstanding part of matching URI.

Pattern: `/actor/:actorQuery`  
Route: `/actor/harrison-ford/movie/enders-game`  
Match: `actorQuery: 'harrison-ford/movie/enders-game'`  

`:param` - optional param, route parameter `param` will hold value of matching URI bit if it exists.

Pattern: `/actor/:actor/:movie`  
Route: `/actor/harrison-ford/enders-game`  
Match: `{ actor: "harrison-ford", movie: "enders-game"`  

##### Retrieving route parameters

Lets assume following page routing configuration.

```js
function($routeProvider) {
  $routeProvider.when('/page/:pageId', {
		templateUrl: 'partials/page.html',
		controller: 'PageController',
		controllerAs: 'page'
	})
}
```

`ngRouter` module comes with `$routeParams` service which allows to retrieve route parameters.
It can be injected into `PageController` and used to access route `pageId` parameter.

```js
function PageController($scope, $routeParams) {
  this.pageId = $routeParams.pageId
}
```

## HTML History API

### `$location` Service

`$location` service provides functionality parsing and changing URL in the address bar.
Angular injects service using `$locationProvider` which can be configured during configuration phase.

There are 2 steps necessary for enabling HTML History API, HTML5Mode enabling via `$locationProvider`
and HTML5 base route configuration.

### Configuring HTML5Mode

```
function configure($routeProvider, $locationProvider) {

  // ... route Provider configuration ...

	// enable HTML5 History API
	$locationProvider.html5Mode(true)
}
```

### HTML5 base route

Configure html base adding `<base href="/">` to the header.

```
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <base href="/">
</head>
```
