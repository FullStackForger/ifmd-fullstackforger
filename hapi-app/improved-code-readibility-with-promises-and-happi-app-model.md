# Improved code readibility with promises and hapi app model

Database access methods exposed on Hapi App Model use promises.
That allows to chain those calls avoiding callbacks.


Lets create user object with few properties first.

```
var user = { fname: 'John', lname: 'Smith', foo: 'bar' };
```

Now you can store it in `user` collection by calling `<Model>.validate()` in any following ways.

### Example 1 `then(onResolve, onReject)`

You can pass `new User()` object as a parameter. Method `validate()` 
returns a promise so you can easily use `then(onResolve, onReject)` syntax.

```        
User.validate(new User(user))
    .then(function(data) {
        return User.insert(data);
    })
    .then(function(data) {
        console.log(data);
    })
    .onReject(function(error) {
        reply(Boom.badRequest('Invalid query: ' + error, error));
        throw new Error(error);
    });
```     

### Example 2 `onReject()` and `onFulfill()`

Additionally `onReject()` and `onFulfill()` methods are also allowed if you prefer them.

Notice that you can pass more than one object and it can be either literal object or one created with the `new <Model>({object})` syntax.

```        
User.insert([user, new User(user), user])
    .onFulfill(function(data) {
        console.log(data);
    })
    .onReject(function(error) {
        throw new Error(error);
    });     
```           

### Example 3 - '.bind(scope)'



```
User.validate(user)
    .then(User.insert.bind(User))
    .onReject(function(error) {            
        throw new Error(error);
    });
```            

This is probably the shortest and most readable way. Those few lines are enought to insert user document into the collection.

### Example 5 - short and sweet

**Note:**  
You **DO NOT** have to use `validate()` method when inserting documents into
database. It will be called internally and validation error will be returned.

```
User.insert(user)
    .then(reply)
    .onReject(function(error) {            
        throw new Error(error);
    });
```      

### Example 5 - writing hapi handler

Here is example Hapi App Model used inside of actual hapi handler.

```
{
    method: 'GET',
    path: '/test/user/save',
    handler: function userHandler(request, reply) {        
    
        var user = { fname: query.name, lname: query.name };

        User.insert()
            .then(reply)
            .onReject(function(error) {
                reply(Boom.badRequest('Invalid query: ' + error));
            });                 
    }
}
```