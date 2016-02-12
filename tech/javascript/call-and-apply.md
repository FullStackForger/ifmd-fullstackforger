
# JavaScript - `call()` and `apply()`

Both methods  Object.call()  and Object.apply() allow to execute function in the specified execution scope passed as a parameter.


It has been  illustrated by following example:

```
var father = {
    age : 50,
    celebrateBirthday : function(fPrefix, fPostfix) {
        return fPrefix + ++this.age + fPostfix;
    }
};

var son = {
    age : 22
};

console.log(father.celebrateBirthday(
'Dad is now: ', 'yr old'));

console.log(father.celebrateBirthday.call(
son, 'Son is now: ', 'yr old'));

console.log(father.celebrateBirthday.apply(
son, ['Son is now: ', 'yr old']));
```


Calling `father.celebrateBirthday()` method increments father.age to 51 and return that value. Even though object son doesn’t have that method implemented it can be called within its execution context, which is passed as first parameter to methods `call()` and `apply()` , as shown in lines: 15 and 18. Executing above code will output:

```
Dad is now: 51yr old
Son is now: 23yr old
Son is now: 24yr old
```

## Difference between `call()` and  `apply()`

Methods behave exactly the same way but there is difference in how they have been called. Method  `call()`  requires arguments separated by comma. Method   `apply()`  requires second argument to be an array of arguments that will be passed to executed function.

From practical point of view methods can be used interchangeable and often it comes down to a developer personal preference while dealing with short list of predefined arguments.

JsPerf – JavaScript performance toolkit was used  to compare efficiency of those methods. After checking few tests I decided to create own bit more complete test revision: "[test call vs apply](http://jsperf.com/test-call-vs-apply/13)". At the time of writing this article methods have been tested in IE, Firefox, Chrome and Safari. Check the test for more details.

It appears that while executing methods with no arguments  or passing just one argument call() method outperforms apply(). Results look much different for hundreds arguments passed into the function, where apply() was executed about 6 times faster – that would mean it should be used when it comes down to higher number of arguments passing to the function along with reference to defined execution scope.

## Practical example

On developer.mozzilla.org pages explain quite well how and when to use both methods, so I will just highlight few examples in this post.

### Build-in function - `apply()`

There is number of functions that accept only arguments separated by comma, like: Math.min() or Math.max(). It is not a problem if we dealing with 2 or few more predefined numbers.But what about investigating numbers stored in the array? This is where Object.apply() magic comes handy. Instead of writing the code to loop through the array comparing pairs of numbers, it can be done with one line of code as shown below:

```
var definedArray = [5, 6, 2, 3, 7];
var max = Math.max.apply(null, definedArray);
```

Another example would be **pushing elements from one array into the other**. `Array.push()`  method accepts any number of arguments but what if those are stored in the array as shown below?

```
var array1 = [1, 2, 3, 4, 5],
    array2 = [6, 7, 8, 9, 10];
Array.prototype.push.apply(array1, array2);
console.log(array1);
```

Yet again one line of code was used to push all elements from one array to the other, instead of the looping through all of them and pushing one at the time.

One more example for `apply()` could be method **converting function arguments into instance of an `Array`**.

```
function testArguments() {
    return (arguments instanceof Array);
}
console.log(testArguments(1,2,3));
```

Executing that code will produce `false`. To convert arguments into the array `Object.apply()` should be run from Array.prototype.slice() method (functions as first-class objects in JavaScript) could be used as shown below:

```
function testArguments() {
    return (Array.prototype.slice(arguments) instanceof Array);
}
console.log(testArguments(1,2,3));
```

### Self executing anonymous function - `call()`

Self executing anonymous function is commonly used by JavaScript developers. It is often executed with current scope passed as an argument which is assigned to internal parameter as in code blow.

```
(function (fScope) {
    var scope = fScope;
    /* function code goes here */
})(this)
```

There is however no need of creating new variable referencing execution context. Word this can be used easily instead if call()  method is used on evaluated function.

```
(function () {
    var scope = this;
    /* function code goes here */
}).call(this)
```

Please note that variable assignment is done just for the sake of the example and  this can be called directly inside function body.

Chaining constructors - `call()`

Creating constructor chains in prototypal inheritance is important part of building JavaScript programs. Example below illustrates how objects can be created by constructor functions, with different arguments, build one on the top of another.

```
function Person(fName, fAge) {
	var name = fName,
		age = fAge;
	this.getName = function () {
		return name;
	}
	this.getAge = function () {
		return age;
	}   
}

function Programmer(fName, fAge, fLanguages) {
	var languages = fLanguages;
	Person.call(this, fName, fAge);
	this.getLanguages = function () {
		return languages;
	}
}

Programmer.prototype = Person.prototype;
```

Let’s now create Programmer object to examine properties and prototypal chain.
```
var jsProgrammer = new Programmer('Mark Bevels', 30,
['html', 'js', 'php', 'as3', 'mysql']);

console.log(' --- properties check --- ');
console.log(jsProgrammer.getName());
console.log(jsProgrammer.getAge());
console.log(jsProgrammer.getLanguages());

console.log(' --- instances check ---- ');
console.log(jsProgrammer instanceof Programmer);
console.log(jsProgrammer instanceof Person);
console.log(jsProgrammer instanceof Object);
```

Above code should generate output as below.

```
--- properties check ---
Mark Bevels
30
["html", "js", "php", "as3", "mysql"]
 --- instances check ----
true
true
true
```

`Person` – first constructor function is  executed within context of `Programmer` constructor. That is why   `this.{properties}` are created  inside of  Programmer instance instead of  Person instance. Last line  `Programmer.prototype = Person.prototype` fixes correct prototype chain to enable `instanceof` check against  Person constructor.


References

* https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/Function/apply
* https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/Function/call
