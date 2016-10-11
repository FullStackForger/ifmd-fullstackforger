
## Project structure

```
./client/                           // client application folder
  ./src/                            // client application source code
  ./script/                         // build / distribution js scripts
  ./img/                            // build / distribution images
  ./css/                            // build / distribution styles
  ./index.html                      // SPA entry point
./plugins/                          //
  ./db/               // no index.js (can be added when plugin is externalized)    
    ./mongoose.js                   // mongoose (mongo) connector
    ./redis.js                      // redis connector
    ./db.js                         // db plugin definition   
    ./db.spec.js                    // db.js test specification
  ./user/                           // no index.js file
    ./test/                         // unit test related
      ./userData.mock.json          // user mock data
      ./config.mock.json            // test configuration
      ./user.frame.js               // shared bdd setups, teardowns, helpers
    ./core/                         // plugin settings and configs
      ./db.config.js                // internal plugin configuration (defaults)
      ./validators.js               // authentication validation methods
      ./routes.js                   // routing definitions
      ./common.js                   // common helper methods
    ./routes/
      ./auth.login.js               // /auth/login route configuration
      ./auth.login.spec.js          // auth.login.js test specification
      ./auth.logout.js              // /auth/logout route configuration
      ./auth.logout.spec.js         // auth.logout.js test specification
      ./auth.signup.js              // ...
      ./auth.signup.spec.js         // ...
      ./auth.google.js              // ...
      ./auth.google.spec.js         // ...
      ./auth.facebook.js            // ...
      ./auth.facebook.spec.js       // ...
      ./auth.twitter.js             // ...
      ./auth.twitter.spec.js        // ...
      ./user.me.js                  // ...
      ./user.me.spec.js             // ...
      ./user.profile.js             // ...
      ./user.profile.spec.js        // ...
    ./models/                       // model definitions
      ./user.model.js               // user model
    ./user.js                       // user plugin definition
./app.js                            // app plugin definition
./config.js                         // app global configuration (defaults)
./config.json                       // app config file (optional & .gitignore'd)
./server.js                         // app server
```


## Unit tests

### Database in unit testing

#### Connecting to database

It is obviously not good idea to run unit test on real database.
Much better approach would be creating random database and removing it after each test.

**Mongoose example**

```
// Connect once for all tests
before(() => Mongoose.connect(`mongodb://localhost/maitredhotel_test_auth_service_${Date.now()}`));

// Disconnect after all tests
after(() => Mongoose.disconnect());

// Clean database after each test
afterEach((done) => {
    Mongoose.connection.db.dropDatabase((err, ));
    done();
});
```

<!-- todo: https://github.com/Automattic/mongoose/issues/1469 -->

#### Mocks

**Mongoose example**

Mongoose has built in `populate` method.
Check out Ben Clinkinbeard's writup: [bclinkinbeard/referenced_mongo_json](https://gist.github.com/bclinkinbeard/5171359)


## Resources
 - https://github.com/hueniverse/postmile-api
 -
