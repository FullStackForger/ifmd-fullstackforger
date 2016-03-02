# Angular Crash Course - Part 4<br/>Filters

## What are filters?

Filters are methods formatting displayed data.
[Angular filters](https://docs.angularjs.org/guide/filter) can be used in view templates, controllers or services.

The underlying API is the filterProvider.

## Using filters

In Angular Expression filter follows expression with a pipe sign.
```
{{ expression | filter }}
```

Filters can be combined.
```
{{ expression | filter1 | filter2 | ... }}
```

And they can have arguments passed into them.
```
{{ expression | filter : arg1 : arg2 |...}}
```

## Built-in filters

### `number` filter

Below example will output rounded number: `1545`.
```
<div ng-init="myNumber = 1545.847342">
<span>
{{myNumber}} after using the number filter:
{{myNumber | number }}
</span>
</div>
```

You can pass precision parameter to precision round a number.
```
<div ng-init="myNumber = 1545.847342">
<span>
{{myNumber}} after using the number filter:
{{myNumber | number:2 }}
</span>
</div>
```
Above example will output number `1545.85`

Check `number` [documentation](https://docs.angularjs.org/api/ng/filter/number) for more details.

### `currency` filter

Bellow example will produce £1,250.90. Default currency sign is '$'.

```
<div ng-init="amount = 1250.90">
<span>
Your final amount is:
{{ amount | | currency:"£" }}</span>
</div>
```
Check `currency` [documentation](https://docs.angularjs.org/api/ng/filter/currency) for more details.


### `date` filter

`date` filter comes with three defined formats: `short`, `fullDate`, `mediumDate` and accepts custom format attributes, eg: `yyyy - MMMM - d  H:m:s`

```
<div ng-init="mydate = 1399648945000">
	<p>{{mydate}} in human readable time is:</p>
	<p>either: <b>{{mydate | date: 'medium'}}</b></p>
	<p>or: <b>{{mydate | date: 'yyyy - MMMM - d'}}</b></p>
</div>
```
Check `filter` [documentation](https://docs.angularjs.org/api/ng/filter/date) for more details.

### `lowercase`/`uppercase` filters
```
<div ng-init="mystring = 'String WITH cUsToM casing'">
  <div>lowercased: {{mystring | lowercase}}</div>
  <div>uppercased: {{mystring | uppercase}}</div>
</div>
```
Check `lowercase` [documentation](https://docs.angularjs.org/api/ng/filter/lowercase) or  `uppercase` [documentation](https://docs.angularjs.org/api/ng/filter/uppercase) for more details.

### `json` filter
```
<div ng-init="obj={ a: 1, foo: 'bar' }">
	<pre>{{ obj | json : 2 }}</pre>
</div>
```

Check `json` [documentation](https://docs.angularjs.org/api/ng/filter/json) for more details.

### `limitTo` filter

`limitTo` can be used on both: strings and arrays. Bellow example will display only last two elements of the array.

```
<div ng-init="myarray = [0,1,2,3,4,5]">
<div>
{{myarray}} after applying the filter:
	{{myarray | limitTo:-2}}</span>
</div>
</div>
```

Check `limitTo` [documentation](https://docs.angularjs.org/api/ng/filter/json) for more details.

### `orderBy` filter

Orders a filtered array by the expression predicate, alphabetically for strings and numerically for numbers.

```
<div ng-init="friends = [
  {name:'John', phone:'555-1212', age:10},
  {name:'Mary', phone:'555-9876', age:19},
  {name:'Mike', phone:'555-4321', age:21},
  {name:'Adam', phone:'555-5678', age:35},
  {name:'Julie', phone:'555-8765', age:29}
}]">
  <table class="friend">
    <tr>
      <th>Name</th>
      <th>Phone Number</th>
      <th>Age</th>
    </tr>
    <tr ng-repeat="friend in friends | orderBy:'-age'">
      <td>{{friend.name}}</td>
      <td>{{friend.phone}}</td>
      <td>{{friend.age}}</td>
    </tr>
  </table>
</div>
```

Check `orderBy` [documentation](https://docs.angularjs.org/api/ng/filter/orderBy) for more details.

### `filter` filter

Uses expression and comparator to filter array of elements and returns it back as an array:
```
{{ filter_expression | filter : expression }}
```
**Expression** is a filter object, eg.: `{name: "Luke", profession: "e"}` would filter elements with property `name` containing `"Luke"` and `profession` containing `"e"`. It is possible to search all properties using `$`, eg: `{$: "e"}` would search for elements with any property containing `"e"`. Prefixing value with `"!"` will negate it.

```
<div ng-init="rebels = [
{name: 'Luke Skywalker', profession: 'Jedi', weapon: 'lightsaber', age: 19 },
{name: 'Ben Kenobi', profession: 'Jedi', weapon: 'lightsaber', age:  56},
{name: 'Han Solo', profession: 'smuggler', weapon: 'laser blaster', age: 32 },
{name: 'Chewbacca', profession: 'pilot', weapon: 'bowcaster' , age: 200},
];">

	<div>
		<input type="radio" name="r" ng-model="option" value="$"> Free text
		<input type="radio" name="r" ng-model="option" value="name"> Name
		<input type="radio" name="r" ng-model="option" value="weapon"> Weapon
	</div>
	<div>
		<span>Search:</span>
		<input type="text" ng-model="searchText[option]" ng-disabled="!option">
	</div>
	<ul>
		<li ng-repeat="rebel in rebels | filter: searchText">
			{{ rebel.name }} (age: {{ rebel.age}})
			is a {{ rebel.profession }} fighting with a {{ rebel.weapon }}.
		</li>
	</ul>
</div>
```

Check `filter` [documentation](https://docs.angularjs.org/api/ng/filter/orderBy) for more details.
