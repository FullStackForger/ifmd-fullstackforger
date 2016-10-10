# React/JSX best practicies and style guide

## Naming conventions

### Component naming

```
// Worst
var myComponent = new React.Component({

// Bad
class myComponent extends React.Component {

// Good
class MyComponent extends React.Component {
```

### File naming

* `PascalCase` for file names, eg: 'YourAwesomeComponent.jsx'
* Use `.jsx` extensions for JSX files

## Files and directories organization


### Files and directories in smaller projects

It is quite common and acceptable to organize files by role as shown below.
```
project-folder/
  actions/
    userActions.js
  components/
    pages/
      Signup/
        index.jsx
        signup.css
      Home.jsx
      index.jsx
      Info.jsx
    App.js
    Header.js
    Sidebar.js
  containers/
    pages/
      index.jsx
      Info.jsx
      Home.jsx
      Signup.jsx
  reducers/
    appReducer.js
    userReducer.js
  tests/
    components/
      pages/
        Signup.spec.js
        Home.spec.js
        Info.spec.js
    containers/``
      pages/        
        Info.spec.jsx
        Home.spec.jsx
        Signup.spec.jsx
  routes.js
```

Content of both `./components/pages/index.jsx` and
`./components/pages/index.js` files:
```
export Home from './Home';
export Info from './Info';
export Signup from './Signup';
```

Example usage:
```
import { Home, Info, Signup } from 'react-toolbox/containers/pages';

// your component class
```

**Motivation**  
At first glance above structure seems to be the most intuitive. Whenever
you look for action, component, container, reducer just go to the designated
folder and scan through the files.

**Disadvantage**  
As the project codebase grows, this approach will force you to create number of
files inside of separate directories. More complex is the feature or component,
more files you will have to add into corresponding directories.

### Files and directories in larger projects

```
app/
  App.jsx
  appReducer.js
  routes.js
pages/  
  Home.jsx
  HomeContainer.js        <-  could be merged with Home.jsx
  Home.spec.js
  index.jsx
  Info.jsx
  InfoContainer.js        <-  could be merged with Info.jsx
  Info.spec.jsx
User/
  index.jsx
  userReducer.js
  UserSignup.jsx
  UserSignupContainer.jsx   <-  could be merged with Signup.jsx
  UserSignup.spec.jsx
  userSignup.css
```

## Sources
* https://github.com/airbnb/javascript/tree/master/react
* https://github.com/nusmodifications/nusmods/wiki/JavaScript-and-React-Style-Guide
* https://github.com/react-toolbox/react-toolbox/issues/98
* http://marmelab.com/blog/2015/12/17/react-directory-structure.html
* http://engineering.kapost.com/2016/01/organizing-large-react-applications
