#SenecaJS Cheat Sheet / Barebones

## About this document

This post is a cheat sheet being made while studying Seneca 
[Getting Started](http://senecajs.org/get-started/) guide.

This is **NOT** a documentation but mere highlight of Seneca features.
Think of this document as reference to everything you have already learned from the official Seneca documentation.

> **Important!**  
> Don't blindly copy and paste code from this docs!  
> It won't work! It is all sudo code used for a quick reference 

### Motivation

Seneca is probably one of the best (if not the best) microservice framework for NodeJS. It has great documentaion with a lot of great practical examples available from github.

As a fullstack freelance developer I wanted to have shorter but complete reference highlighting all Seneca's features and that's what this document is intended to be. 

<!--todo: insert contact info and links to microservices posts-->

## SenecaJS Features

### Pattern / action registration

```js
seneca.add(
    {/* pattern */}, 
    function action(msg_obj, response_cb) {     
        /* action logic */
        response_cb(error, response_obj);
    });
```

### Submitting message

```js
seneca.act(
    {/* message (pattern + data) */}, 
    function response_cb(error, response) {
        /* response callback logic */
    });
```

### Pattern matching

#### Definitions

Patterns can be passed as an object literal
```
seneca.act(
    {role: 'math', cmd: 'sum', left: 1.5, right: 2.5},
    respond_cb 
);   
```

or as a string literal (see [jsonic](https://github.com/rjrodger/jsonic))
```
seneca.act(
    'role: math, cmd: sum, left: 1.5, right: 2.5',
     respond_cb 
);        
```

or as a combination of both
```
this.act(
    'role:math,cmd:sum', 
    {left:  1.5, right: 2.5}, 
    respond_cb 
);
```

#### Rules

 * more matching attributes wins
 * alphabetical ordering for patterns with same number of attributes

#### Examples

 * `a:1, b:2` wins over `a:1` with more properties
 * `a:1, b:2` wins over `a:1, c:3` with alphabetical order
 * `a:1, b:2, d:4` wins over `a:1, c:3, d:4` with alphabetical order
 * `a:1, b:2, c:3` wins over `a:1, b:2` with more properties
 * `a:1, b:2, c:3` wins over `a:1, c:3` with more properties

### Pattern overriding

Example registered action pattern:
```js
seneca.add({/* pattern A */}, function (message, response_cb) {
    /* action logic */   
    response_cb(err, result);
});
```
can be overriden as shown below:
```js
seneca.add({/* pattern A */}, function (message, response_cb) {
    /* additional action logic, error handling, message mutations */    
    this.prior(
    { /* message (including pattern A) */}, 
    function internal_resposnse_cb(err, result) {
        /* result mutations, error handling */
        response_cb(err, result);
    }) 
});
```
**Note:**  
'this' keyword is a reference to seneca object.

The `this.prior()` method takes two parameters:
* modified (or not) message object
* internal response callback function used to modify result

PLugin patterns can be overriden with wrapping actions, check Plugins section below for more information.

### Plugins

#### Default plugins

Seneca loads four built-in plugins by default: [basic][p-basic], [transport][p-trans], [web][p-web] and [mem-store][p-store].

[p-basic]: https://www.npmjs.com/package/seneca-basic
[p-trans]: https://www.npmjs.com/package/seneca-web
[p-web]: https://www.npmjs.com/package/seneca-web
[p-store]: https://www.npmjs.com/package/seneca-mem-store

#### Loading custom plugin

```
require( 'seneca' )().use( customPlugin );
require( 'seneca' )().use( customPlugin, { /* options */ } );
```

#### Creating Custom plugins
```
function customPlugin( options ) {
    // use initialization pattern to init
    this.add( 'init:math', function init( msg, respond ) {
        // custom init logic (eg.: logging)
        respond()
    });
    // register rules with this keyword
    this.add( 'custom:pattern', function customAction( msg, respond ) {
        respond( err, response_ob   j );
    })
}
```

##### Plugin initialization
Actions registered with `init:customPlugin` pattern are used to initialize a plugin. Init actions are called in sequence for each plugin. Initializing action methods require `respond()` call without errors. 

##### Action signatures
Defining action signatures are logged, eg: `customAction` is explicitly defined in line: `this.add( 'custom:pattern', function customAction( msg, respond ) {`

#### Overriding plugin action patterns by wrapping

```
function customPlugin (options) {
  this.add( 'role:math,cmd:sum', function sum_action( msg, respond_cb ) {
    /* calculate result */
    respond_cb( null, result );
  });
  this.add( 'role:math,cmd:product', function prod_action(msg, respond_cb ) {
    /* calculate result */
    respond_cb( null, result );
  });
  this.wrap( 'role:math', function extension_action( msg, respond ) {
    /* additional custom logic */
    this.prior( msg, respond )
  });
}
```

Set of matching patterns can be overriden with `seneca.wrap` method, which takes two parameters:
* pattern-matching pin (string or object)
* action extension function

### Microservices

#### Loading external plugins
```
require( 'seneca' )().use( require('./customPlugin.js') );
```
altenartively
```
require( 'seneca' )().use( 'customPlugin' );
```

The `seneca.use` method will attempt to auto require `Custom Plugin` from the local folder.

#### Staring microservice
The `listen()` method starts microservice process listening on port 10101 for incomming HTTP requests used to transport messages.
```
require( 'seneca' )()
  .use( 'customPlugin' )
  .listen()
```

to test use browser
```
http://localhost:10101/act?role=math&cmd=sum&p1=1&p2=2
```

or curl
```
curl -d '{"role":"math","cmd":"sum","p1":1,"p2":2}' http://localhost:10101/act
```

**Note:**  
Either way `act` method has to be appended to url.


**Note:**
On windows machines, host has to be set to `localhost`. By default client will try to connect to host at 0.0.0.0 (and it won't work).

#### Starting microservice client

```
require( 'seneca' )()
  .client()
  .act('role:math,cmd:sum,p1:1,p2:2', console.log)
``` 
#### Client and service parameters

 * port: optional integer; port number.
 * host: optional string; host IP address.
 * spec: optional object; full specification object.

Client / Listen example:
```
seneca.client( { port:8080, host:'192.168.0.2', port: 1111 } )
seneca.listen( { port:8080, host:'192.168.0.2', port: 1111 } )
```

#### Transport (independence) layer

Seneca built-in [transport](https://www.npmjs.com/package/seneca-transport) allows comminication over both HTTP (default) and TCP protocols.

The default client/listen configuration sends all messages that the client does not recognize over the listening server

**Recommended practice:**  
Directing communicatoin of action patterns can be done with `pin` option:
```
seneca.client( {type:'tcp', pin:'action:math' } )
seneca.listen( {type:'tcp', pin:'action:math' } )
```


### Logging

All logs
```
node program.js --seneca.log.all
```

Plugin logs
```
node program.js --seneca.log.all | grep plugin | grep DEFINE
```

Renders visual tree of patterns
```
node program.js --seneca.print.tree
```

Prints single plugin logs
```
node program.js --seneca.log=plugin:math
```

#### Log attributes (columns)

* date-time: when the log entry occurred.
* seneca-id: identifier for the Seneca process.
* level: one of DEBUG, INFO, WARN, ERROR, FATAL.
* type: entry code, such as act, plugin, etc.
* plugin: plugin name (actions without a plugin have root$).
* case: entry case, such as IN, OUT, ADD, etc.
* action-id/transaction-id: tracing identifier, constant over the network.
* pin: the action pattern for this message.
* message: the inbound or outbound message (truncated if too long).


## Unanswered questions

1. How to pass options into the plugin?

