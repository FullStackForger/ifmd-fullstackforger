# Angular Crash Course - Part 1<br/>Setting up Angular project

## What is Angular?

Angular is an enhanced HTML for web apps.

Angular is a framework allowing developer to construct application in a [declarative](https://en.wikipedia.org/wiki/Declarative_programming) manner.
It automates number of tedious tasks:
- application initialization / bootstrap
- DOM manipulation
- callback registration
- populating views with data

Source: (Angular Introduction](https://docs.angularjs.org/guide/introduction)

## Keeping up to date with AngularJS

- Checkout official [angularjs.org](https://angularjs.org/) site for framework overview, [tutorial](https://docs.angularjs.org/tutorial), [developer guide](https://docs.angularjs.org/guide), [API docs](https://docs.angularjs.org/api) and more.
- Read [angularjs.blogspot.co.uk]( http://angularjs.blogspot.co.uk/) - official AngularJS blog to get fresh updates.
- On Twitter follow official [AngularJS](https://twitter.com/angularjs) channel for updates.
- Follow [IndieForger](https://github.com/indieforger) on github for JS full stack code examples.

## Project setup

### Top level directory structure

```
mkdir app       # production code
mkdir test      # test code
mkdir dist      # distribution code
```

### Git configuration

Either `git clone` Angular [seed](https://github.com/angular/angular-seed) application or `git init` if you start from scratch.

### NPM and packages
```
npm init
npm install -g bower
```

#### Bower
```
bower init
bower install angular --save
bower install angular-resource --save
bower install angular-route --save
```

Add `bower_components` to `.gitignore`.

Check [official docs](http://bower.io/docs/config/) to see how to modify default bower configuration.

#### Karma
```
npm install -g karma            # installed for cli convenience
npm install karma --save-dev
npm install karma-jasmine --save-dev
npm install karma-chrome-launcher --save-dev
```
