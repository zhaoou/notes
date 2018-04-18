## Router

### foundations

- router mimics browser functionality by watching and modifing URL address
- BrowserRouter: listens to URL changes and renders the UI

### installation

- `import { BrowserRouter } from 'react-router-dom'`
- Wrapp app `<BrowserRouter><App /></BrowserRouter>`

### Key components

```
import {BrowserRouter, Switch, Route, Link, NavLink} from "react-router-dom";
```


#### BrowserRouter

- enables routing

```
ReactDOM.render(
  <BrowserRouter><App/></BrowserRouter>,
  document.getElementById('root'));
```


#### Route

- connects URL with component
- can have one of (render or component) attributes

```
<Route exact path="/" render={ ()=> (<SomeComponent/>) } />
<Route path="/create" component={SomeComponent} />
```


#### Switch

- prevents 'fallthrough' - rendering of two mutually exclusive components

```
<Switch>
  <Route path="/about" component={About} />
  <Route path="/" component={Home} /> 
</Switch>
```


#### Link

- clicking Link tells BrowserRouter to update URL
- `<Link to="/create"/>`
- with payload:
```
<Link to={{       
  pathname: '/courses',
  search: '?sort=name',
  hash: '#the-hash',
  state: { fromDashboard: true }    }}> Courses </Link>
```


#### NavLink

- a link designed for navigation
- accepts default and active styles
```
let active = { color: "red"};
let defaultS = {margin: "5px"};
`<NavLink exact activeStyle={active} style={defaultS} to="/">home</NavLink>`
```



- `history.push("/");` forces the app to go to the main route => router will render whatever component is mapped to `"/"`
