# Angular Crash Course - Part 3<br/>Directives

## What are Angular Directives?

Directives are html markers, (instructions) informing angular compiler to attach behavior or transform DOM element.
Check out [official docs](https://docs.angularjs.org/guide/directive) for more.

### Directive types

Angular compiler (`$compile`) can match directives based on element names, attributes, class names, as well as comments, eg:
```
<my-dir></my-dir>
<span my-dir="exp"></span>
<!-- directive: my-dir exp -->
<span class="my-dir: exp;"></span>

```

### Normalization

During compilation Angular will normalize encountered directives into [camelCase](https://en.wikipedia.org/wiki/CamelCase) format.
Additionally it will strip away `x-` and `data-` prefixes that can be used for html validation checks in older browsers.

```
<div ng-controller="Controller">
  Hello <input ng-model='name'> <hr/>
  <span ng-bind="name"></span> <br/>
  <span ng:bind="name"></span> <br/>
  <span ng_bind="name"></span> <br/>
  <span data-ng-bind="name"></span> <br/>
  <span x-ng-bind="name"></span> <br/>
</div>
```

## Custom angular directives

### `ng-app`
It is directive bootstrapping angular application, usually attached to `<html>` or `<body>` element
```
<div ng-app>
  <p>{{1+2}}</p>
</div>
```

### `ng-href`
```
<a ng-href="http://test.com/{{username}}">
```

### `ng-src`
```
<img ng-src=”http://test.com/{{username}}.jpg”>
```

### `ng-model`
```
<p>Hello, {{user}}!</p>
<input type="text" ng-model="user">
```

### `ng-init`
```
<div ng-init="name = 'John'">
     Hello {{ name }}
</div>
```

### `ng-disabled`
```
<div>
  Your email:
  <input type="text" ng-model="emailAddress">
</div>
<div>
  <input type="submit" ng-disabled="!emailAddress">
</div>
```

### `ng-bind` and `{{` + `}}`

```
<div ng-init="name = 'Darth Vader'">
  <p ng-bind="name"></p>
</div>
```

```
<div ng-init="name = 'Darth Vader'">
<p>{{name}}</p>
</div>
```

### `ng-click`
```
<div ng-init="number = 0">
	<p>My number: {{number}}</p>
  <button ng-click="number = number + 1">+1</button> <button ng-click="number = number - 1">-1</button>   
</div>
```

### `ng-options`

```
<div ng-init="staff = [
    {name: 'Tom', profession: 'Developer', age: 23 },
    {name: 'John', profession: 'Designer', age:  25},
    {name: 'Peter', profession: 'Manager', age: 24 }
];"></div>
<select ng-model="employee" ng-options="person.name for person in staff">
  <option value="">Select an employee</option>
</select>
<p>Selected: {{ employee.name }}  ({{ employee.profession }})</p>
```

### `ng-show`, `ng-hide`
```
<div ng-init="person = { name: 'John Smith', age: 20 }">
	<p>How old is {{person.name}}?</p>
  <input type="number" ng-model="ageGuess">
	<p ng-hide="person.age !== ageGuess">Correct</p>
	<p ng-show="person.age !== ageGuess">Incorrect</p>
</div>
```



### `ng-class`
```
<style>
div > p { display: none; }
.red { color: red; display: block; }
.green { color: green; display: block; }
</style>
```

```
<div ng-init="person = { name: 'John Smith', age: 20 }">
	<p>How old is {{person.name}}?</p>
	<input type="number" ng-model="ageGuess">
	<p ng-class="{green: person.age === ageGuess}">Correct</p>
	<p ng-class="{red: person.age !== ageGuess}">Incorrect</p>
</div>
```

### `ng-repeat`
```
<ul>
<li ng-repeat="number in [0,1,2,3,4,5,6,7,8,9]">
Element number: {{ number }}
</li>
</ul>
```

#### Combine with `ng-class`
```
<style>
.odd { color: blue; }
.even { color: green; }
</style>    
```

```
<ul>
<li ng-repeat="number in [0,1,2,3,4,5,6,7,8,9]"
 ng-class="{even: $even, odd: $odd}">
Element number: {{ number }}
</li>
</ul>
```

### `ng-cloak`
```
<style>
[ng\:cloak], [ng-cloak], .ng-cloak {
display: none !important;
}
</style>   
```

```
<ul ng-cloak>
  <li ng-repeat="number in [0,1,2,3,4,5,6,7,8,9]">
    Element number: {{ number }}
  </li>
</ul>
```
