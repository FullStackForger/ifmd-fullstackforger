# Angular Crash Course - Part 2<br/>Angular bootstrap

## Hello world

Create `index.html` with sample html.

```
<!doctype html>
<html lang="en" ng-app>
<head>
	<meta charset="utf-8">
</head>
<body>
<p>{{'Angular' + ' ' + 'Bootstrap'}}</p>
<script src="bower_components/angular/angular.js"></script>
</body>
</html>
```

`<script src="bower_components/angular/angular.js"></script>`

## Bootstrap directive
Used in the example above `ng-app` is a [bootstrap directive](https://docs.angularjs.org/api/ng/directive/ngApp),
used to auto-bootstrap an AngularJS application. It is typically placed near the root element of the page on the `<body>` or `<html>` tags.

## Angular expressions
[Angular expressions](https://docs.angularjs.org/guide/expression) are JavaScript-like code snippets facilitating designing view templates.
In the example above we used `{{'Angular' + ' ' + 'Bootstrap'}}` expression which renders concatenated strings.

### Allowed Angular expressions
```
1+2
a+b
user.name
items[index]
test = 123
```

### Angular expressions vs JavaScript expression

- &#10004; Forgiving to `undefined` and `null`  
- &#10004; Can be used with [Angular Filters](https://docs.angularjs.org/guide/filter)
- &#10004; Evaluated in the context of local object scope

- &#10008; No flow control statements
- &#10008; No function declarations
- &#10008; No RegExp
- &#10008; No comma and void operators
