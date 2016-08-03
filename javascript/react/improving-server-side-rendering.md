# Improving React Server Side Rendering

Tips from [Reactjs - Speed up Server Side Rendering - Sasha Aickin](https://youtu.be/PnpfGy7q96U)

## Production mode

```
 node_env = 'production'
```
Setting up production mode reduces error checking ReactJS performs in development mode

## Minified react version

Replace
```
import ReactDOMserver from "react-dom/server"
```

with
```
react/dist/react.min
```

> **Important**  
> It will make server harder to debug that is why you shouldn't be doing it in development.

## Babel transforms

```
npm install --save-dev \
 babel-plugin-transform-react-constant-elements \
 babel-plugin-transform-react-inline-elements
```

Add them to babel config file.

```
"presets": [ /** **/ ]
"plugins": [
  "transform-react-constant-elements",
  "transform-react-inline-elements"
]
```
> **Important**  
> 1. **constant** plugin should be added as the first one.
> 2. Requires babel build step



## Avoid `React.createClass()`

Replace all instances of `React.createClass()`, eg:
```
var MyCustomClass = React.createClass({
  render: function {
    return <div>Hello world</div>
  }
})
```
with:
```
class MyCustomClass extends React.Component {
  render() {
    return <div>Hello world</div>
  }
}
```

> **Important**  
> 1. Refactoring code requires a bit more time.
> 2. It introduces few gotchas that might generate bugs, for example auto bound handlers.
<!-- todo: go through click method again from the video -->

## Streaming

Chunked Encoding available from HTTP/1.1 (1999) allows to serve partial HTML to the browser.
There is a [aickin/react-dom-stream](https://github.com/aickin/react-dom-stream) - a streaming server-side rendering library for React.
It allows to replace `ReactDOM.renderToString` with `ReactDOMStream.renderToString`
and use `ReactDOMStream.renderToStaticMarkup` alongside to provide readable streams
as children in the React element tree the same way that it accepts Strings as children in the tree.

Check out Sasha's project for more details, including **component caching** feature.
