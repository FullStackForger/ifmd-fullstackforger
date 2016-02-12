
Coming to the world of JavaScript from different object oriented programming language might make you feel confused about object models along with inheritance and few other. Personally, it took me some time to get my head around JavaScript, hence this post. It will highlight everything that seemed important or interesting my JavaScript journey.

JavaScript seems to be easy to learn at first but if you happened to be inquisitive programmer and ask around, you will find out that many developers don't really grasp the true nature of the language. It is important to understand that ability of copying \*.js script from the Internet and pasting it into HTML document or even using libraries like JQuery doesn’t really make good programmer.

Lets get started.

## Variables

### Types of variable

There are only 5 different types of variable:

`boolean` – to store true or  false value (1 or 0)
`number` – to store integer or real number, both positive and negative are allowed,
`string` – used to store string of characters, can be assigned as empty string,
`function` – used to break program into logical functionality chunks,
`object` – used to encapsulate variables and other functions into coherent and reusable bundle , JavaScript is loosely typed language. It means that variable type is defined at the time of assigning value and can be changed dynamically from one type to the other.

### Variable declaration and variable assignment
A  variable declaration defines named variable. It starts with  var and even though it is not compulsory, usually is  followed by variable assignment, ex:

```
var myBoolVariable = true;
var myStringVariable = 'my test variable value';
var myIntegerVariable = 2;
var myFunctionVariable = function() {};
var myObjectVariable = {};
var myUndefinedVariable;
```

Recommended practice is assigning variables at the top of current context scope with use of one  var word, separating declared variables with comma. Following that practice above example should be refactored as shown in the code below:

```
var myBoolVariable = true,
    myStringVariable = 'my test variable value',
    myIntegerVariable = 2,
    myFunctionVariable = function() {},
    myObjectVariable = {},
    myUndefinedVariable;
```

### Operator: typeof

Operator `typeof` is used to check type. Lets check types of already defined variables:

```
console.log(typeof myBoolVariable);
console.log(typeof myStringVariable);
console.log(typeof myIntegerVariable);
console.log(typeof myFunctionVariable);
console.log(typeof myObjectVariable);
console.log(typeof myUndefinedVariable);
```

Expected console output:

```
boolean
string
number
function
object
undefined
```

## Functions

Functions are central and critical part of the language, allowing break whole program into smaller logical pieces of functionality.

### Function declaration
There are three ways function can be defined:

```
function function1(fParam) { return "fParam: " + fParam; };
console.log(function1('text value')); // -> fParam: text val

var function2 = function(fParam) { return "fParam:  " + fParam; };
console.log(function2('text value')); // -> fParam: text value

var function3 = new Function("fParam", "return \"fParam: \" + fParam;");
console.log(function3('text value')); // -> fParam: text value
```

**Important:**  
In JavaScript function is a Function object and that is why function instances inherit from  Function.prototype.

### Function scope

Lets look at below example function:

```
function Func() {
    var scope = this;                    // ! important !
    this.scope = scope;
    return scope;
}
```

If function is created with the Function constructor, closures scope for its context is not created, instead it runs in the window context. In other words executing function will happened inside current scope. Paste below example into your Chrome or Firefox developer console and you will see  `[object Window]`

```
console.log(Func().toString());          // -> [object Window]
```

However if defined function is used as constructor for creating new object it creates closure scope for own context. That is illustrated below.

```
var func = new Func();
console.log(func.scope.toString());     // -> [object Object]
console.log(func.scope === func);       // -> true
```

Log in line 3 will return true which proves that function.scope  points to itself. And that proves that  internal scope of its context has been created and can be accessed with word this.

## Hoisting

Hoisting is a process of moving variable declaration or function declaration to the top of current context scope. It is operation performed by JavaScript interpreter before running the code of current execution scope.

### Variable hoisting

```
var testVariable  = 'defined outside';
function printVariable() {
    return testVariable;
    var testVariable = 'defined inside';
}
console.log(printVariable());
```

Executing this example will output  undefined in the console. It happens because variable declaration is hoist to the top of function execution context, and code is run as shown below:

```
var testVariable  = 'defined outside';
function printVariable() {
    var testVariable;
    return testVariable;
    testVariable = 'defined inside';
}
console.log(printVariable());
```

In process of moving variable declaration to the top (line 2) it becomes available before it’s actually expected to be initiated from the code. It is important to notice that variable assignment still takes place as expected, thus function returns  undefined value as private testVariable hasn’t been defined before `return` statement.

Variable hoisting only occurs only if var keyword is found inside the context. Lets confirm this behavior by removing var from previous example.

```
var testVariable  = 'defined outside';
function printVariable() {
    return testVariable;
    testVariable = 'defined inside';
}
console.log(printVariable());
```

The console will log defined outside as expected.

### Function hoisting

Function hoisting works the same way as variable hoisting if function has been defined using variable declaration with keyword var. If however function is defined with function declaration whole function body will get hoisted along. It can be examined by executing two code examples shown below.

#### Example 1

```
function getVarData() {
    return funct();
    var funct =  function() { return 1; }
}
console.log(getVarData());
```


#### Example 2

```
function getFunctData() {
    return funct();
    function funct() { return 2; }
}
console.log(getFunctData());
```

Running first block of code error will produce an error: `TypeError: undefined is not a function`. Second block can be executed successfully logging expected number: 2.

## Closures

JavaScript like other object oriented programming languages allows creating objects inside other objects. It does even take it further and because function is an object it can be created inside another function scope.

Created that way function is called `closure`.  It will have own context allowing private variables and additionally will  have full access to outer scope and its private variables acting as privileged method. It is even better than it sounds. Closure has ability to access private variables from the scope of outer execution context even after itself has been returned as a value.

Self executing anonymous function
Lets consider below example:

```
var elArr = [ /* DOM Collection */ ];

for (var i = 0; i < elArr.length; i++) {
    elArr[i].onclick = function() {
        console.log( 'clicked element index: ' + fIndex);
    };
}
```

In above case it doesn’t matter which element has been clicked. console log will always display "clicked element index: n"  where n  is the last index in `elArr` array. Why?

Context of anonymous function assigned to `elArr[i].onclick` runs within main scope and by the time any element will be clicked last will already be assigned index to i  variable. This is where closure comes very handy.

```
var elArr = [ /* DOM Collection */ ];

for (var i = 0; i < elArr.length; i++) {
    elArr[i].onclick = (function (fIndex) {
        return function() {
            console.log( 'clicked element index: ' + fIndex);
        }
    }(i));
}
```

In last example current loop index has been passed into internal context of the closure by self-executing (line 8) anonymous function (line 4). Clicking element from `[ /* Dom Collection */]` array will log correct index.

Closures are commonly used to create scope of privileged methods being able to access private variables from within their execution context. By wrapping code in self-executable function it is possible to access and manipulate its private variables via exposed public methods, which is called Modular Pattern.

## Objects

Objects are binds of functionality  bundles, defined as functions, into reusable package that can be reference as one entity rather than many. They are hash tables of key – value pairs.

JavaScript is object oriented language and that is why everything revolves around Object. Almost everything in JavaScript is or like as an object with exception of `null and undefined`.  What does it mean? Lets use instanceof to find out (  `instanceof` operator will be explained later).

```
console.log([] instanceof Object);                // -> true
console.log(function(){} instanceof Object);      // -> true
console.log(new Object() instanceof Object);      // -> true
```

All above statements logs true because in JavaScript arrays are objects, functions are objects and objects are objects.

### Creating objects

In classical object oriented languages objects are described by and are instances of classes. In JavaScript objects are created by constructors. Those are functions used to initialize objects and provide the features that can be used as mentioned classes in other languages, including private, public or static variables and methods.

Objects can be created in three different ways: Object.create() method, function Object as constructor and literal  by using '{' and '}'. Below statements are equivalent.

```
var obj1 = Object.create(Object.prototype);
var obj2 = new Object();
var obj3 = {};
```

By default newly created  objects already has few methods like:

```
Object.prototype.constructor()
Object.prototype.toString()
Object.prototype.toLocaleString()
Object.prototype.valueOf(V)
Object.prototype.hasOwnProperty(V)
Object.prototype.isPrototypeOf(V)
Object.prototype.propertyIsEnumerable(V)
```

However in some cases you might want to design empty object. Those are three equivalent methods, as used above to empty object wrapper:

```
var obj1 = Object.create(null);

var obj2 = new Object();
obj2.__proto__ = null;

var obj3 = {};
obj3.__proto__ = null
```

In first example object has been created by passing null parameter into Object.create()  method, which creates empty wrapper. With using Object  constructor function or {} object prototype had to be nullified.

### Object as associative array

Standard way of accessing object properties is dot  notation ex.: object.property. It is however possible to access object properties as element of associative array using property name as key to the value, ex.: `object[property]`.  Consider following object:

```
var obj = {
    p1 : 'first property',
    p2 : 2,
    p3 : [1, 2, 3],
    p4 : { name: 'internal object' },
    m1 : function() { return 'first method()'; },
    m2 : function() { return 'second method()'; }
}
```

You can access properties with dot operator. For example property `p1` can be accessed by `obj.p1` or with key as in associative array `obj['p1']` . Same applies for methods and so method m1 can be called by `obj1.m1()` or `obj['m1']()`.

Because object can be accessed as associative array  it is possible to loop through its properties and methods using  `for ( key in obj) { }`.

Execute below code for already created `obj`  object.

```
for (var i in obj) {
    console.log(i + ': ' + typeof(obj[i]));
}
```

And console.log will output:

```
p1: string
p2: number
p3: object
p4: object
m1: function
m2: function
```

### Object literal
As shown already it is possible to create the object using literal notation –  { }. Object created that way inherits from Object.prototype and because function Object has been used as its constructor it does have own context scope as explained in Function scope section.

```
var object = {
    description : "object created  used literal notation",
    getScope : function() { return this; }
}
console.log(object.__proto__.toString());      // -> [object Object]
console.log(object.getScope().toString());     // -> [object Object]
```

**Note:**  
Code in line 5 logs Object prototype and in line 6  logs object itself and in both cases same object is returned, that is why below statements are true.

```
Object.prototype === object.__proto__                    // -> true
Object.prototype === object.getScope().__proto__         // -> true
object.getScope() === object                             // -> true
object.getScope().getScope() === object                  // -> true
```

## Prototypal inheritance

“Prototype” and “prototypal inheritance” should, both be a part of Object section but because the complexity of the subject it earned the title of separate topic.

### Prototype definition

Prototype means preliminary model on which something else is based or created – this definition does apply to JavaScript prototype as well. Prototype is an object created, with constructor property referencing declared function when that is being declared.

Prototype is “base” object shared between other objects if its constructor property had been used to create them. It becomes integral part of  so called prototype chain (explained later in this section) and is used for property lookup.

### Prototype property

```
function MyObject(fName) {
    var name = fName;
    this.getName = function () { return name; }
}

var obj1 = new MyObject('1st object'),
    obj2 = new MyObject('2ne object');
```

Constructor `function MyObject` was defined along with  `prototype` property which is an object with internal property called constructor – referencing function itself. That can be tested by code below:

```
MyObject.prototype.constructor === MyObject
```

### `instanceof` operator

Last too lines of  previous example create two objects (`obj1`  and  `obj2`) using MyObject constructor function. Both of them have MyObject in prototype chain. It can be checked with `instanceof` statement returning true for all below checks:

```
console.log(obj1 instanceof MyObject);
console.log(obj1 instanceof Object);
console.log(obj2 instanceof MyObject);
console.log(obj2 instanceof Object);
```

`instanceof`  is  a binary operator with following syntax:  `instance instanceof Constructor`. It checks object against constructor function and return true if object was created using that constructor.

### Modifying shared prototype (at runtime)

As it was said already, prototype is shared between other objects if prototype.constructor function has been used to create them.

In JavaScript objects can be modified during the runtime. Because prototype objects fall into regular object category, they can be modified at runtime too. Change performed on prototype object its will be reflected in its descendants.

Using defined function from the first example  from this chapter, lets try to modify `MyObject.prototype`  and check  how does it affect created objects.

```
MyObject.prototype.getLabeledName = function () {
    return 'object name: ' + this.getName();
}

console.log(obj1.getLabeledName());
console.log(obj2.getLabeledName());
```

Because we modified prototype shared by both objects console will output:

```
"object name: 1st object"
"object name: 1st object"
```

### `__proto__` and prototype properties

`prototype`  is an internal property of a Function object and the prototype of objects constructed by that function.

`__proto__` is internal property of an object, referencing its its prototype. Even though as a property is accessible for a developer it is not a good practice to modify it.

Below example will return true showing dependency between __proto__ and .prototype.

```
var object = new Object();
object.__proto__ === Object.prototype
```

### Prototypal inheritance

JavaScript doesn’t have classes, which at first might seem to be limitation for language capabilities. The true however is that it makes JavaScript more flexible Object Oriented language comparing to some of the other mainstream languages.

Because of no classes JavaScript uses prototype as a construct to store information about objects properties and methods describing behavior of other objects. That is called prototypal inheritance.

Every newly created objects has own prototype chain. It is used to property look up. Firstly object itself will be examined whether it defines the property on its own.  If property is not found its prototype will be search up to the top of prototype chain hierarchy or until property is found.

```
var father = { name: "John", surname: "Smith" };
var son = { name: "Ted" };

father.__proto__ = new Object({
    getFullName : function() {
        return this.name + ' ' + this.surname;
    }
});

son.__proto__ = father

console.log(father.getFullName());
console.log(son.getFullName());
```

Even though modifying __proto__ property is not considered the best practice, in above example it is the only way to change object prototype as it is not created with constructor function. Code will return:

```
John Smith
Ted Smith
```

Lets examine the code:


line 1: father literal object is created  with two properties (name, surname);
line 2: son literal object is created, with name property;
line 4: new object is assigned to `father.__proto__`  with own prototype  passed as a function parameter, containing  getFullName function property;
line 10: father object is assign to `son.__proto__`  making son inheriting father properties

**Note:**  
Assigning new object to `father.__proto__`  was necessary. Adding getFullName function property directly to `father.__proto__`  would change Object.prototype . That means all objects would have getFullName property in their prototype chain.

## Resources

https://developer.mozilla.org/en-US/docs/JavaScript
http://www.w3schools.com/js/
http://bonsaiden.github.com/JavaScript-Garden/
http://kangax.github.com/es5-compat-table/#strict-mode-ie10
http://joost.zeekat.nl/constructors-considered-mildly-confusing.html
