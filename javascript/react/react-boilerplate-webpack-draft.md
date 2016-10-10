
https://github.com/gaearon/react-hot-boilerplate

## Setup

### Project structure

Create directories and initial files.
```
mkdir react-bootstrap
cd react-bootstrap
touch webpack.config.js
mkdir src
touch index.html src/index.js src/app.js
```

Let's take a look on what goes where.
```
./dist/                 <- build folder
./src/                  <- source folder
./src/components/       <- where we keep the code
./src/index.js          <- application bootstrap
./src/app.js            <- application component
./index.html            <- main html file
./.babelrc              <- babel options
./webpack.config.js     <- webpack configuration file
```
### Dependencies

Init NPM and load dependencies.
```
npm init
npm i -D react react-router react-hot-loader
npm i -D webpack webpack-dev-server
npm i -D babel-core babel-loader babel-preset-{react,latest}
```

We have installed number of dependencies. Let's go through them.

**React**
* [react][react] - main reason you are reading this
* [react-router][react-router] - routing library for react
* [react-hot-loader][react-hot-loader] - allows dynamic component reloading

**Webpack**
* [webpack][webpack] - js module bundler
* [webpack-dev-server][webpack-dev-server] - [Express][express] server,
which uses the [webpack-dev-middleware][webpack-dev-middleware] to serve a

**Babel**
* [babel-core][babel-core] - [Babel][babel] js compiler core
* [babel-loader][babel-loader] - webpack plugin for Babel
* [babel-preset-react][babel-preset-react] - react modules including (jsx, flow, etc.)
* [babel-preset-latest][babel-preset-latest] - latest ES2015+ syntax modules

## Configuration

todo...

<!-- references -->

[babel]: https://babeljs.io/
[babel-loader]: https://github.com/babel/babel-loader
[babel-core]: https://github.com/babel/babel/tree/master/packages/babel-core
[babel-preset-react]: http://babeljs.io/docs/plugins/preset-react/
[babel-preset-latest]: https://babeljs.io/docs/plugins/preset-es2015/
[express]: http://expressjs.com/
[react]: https://facebook.github.io/react/
[react-router]: https://github.com/reactjs/react-router
[react-hot-loader]: https://github.com/gaearon/react-hot-loader
[webpack]: https://webpack.github.io/
[webpack-dev-server]: https://webpack.github.io/docs/webpack-dev-server.html
[webpack-dev-middleware]: https://github.com/webpack/webpack-dev-middleware
