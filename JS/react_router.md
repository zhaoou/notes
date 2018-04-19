# Router

## Foundations

- router mimics browser functionality by watching and modifing URL address
- BrowserRouter: listens to URL changes and renders the UI

## Installation

- `import { BrowserRouter } from 'react-router-dom'`
- Wrapp app `<BrowserRouter><App /></BrowserRouter>`

## Key components

```
import {BrowserRouter, Switch, Route, Link, NavLink} from "react-router-dom";
```


### BrowserRouter

- enables routing

```
ReactDOM.render(
  <BrowserRouter><App/></BrowserRouter>,
  document.getElementById('root'));
```


### Route

- connects URL with component
- can have one of (render or component) attributes

```
<Route exact path="/" render={ ()=> (<SomeComponent/>) } />
<Route path="/create" component={SomeComponent} />
```


### Switch

- prevents 'fallthrough' - rendering of two mutually exclusive components

```
<Switch>
  <Route path="/about" component={About} />
  <Route path="/" component={Home} /> 
</Switch>
```


### Link

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


### NavLink

- a link designed for navigation
- accepts default and active styles
```
let active = { color: "red"};
let defaultS = {margin: "5px"};
`<NavLink exact activeStyle={active} style={defaultS} to="/">home</NavLink>`
```

## Dynamic routing

- enables path variables

```
<Route path="/:name" component={Home} />

let Home = ({match}) => (<h1> Hello {match.params.name}</h1>);

```

### Route props

- any component rendered inside of Route component get 3 props: match, location, and history
- if we need this props in a component NOT rendered inside of a Route, use `withRouter()` function

```
// accepts and uses history prop to create a button pointing home

let Routes = ({history}) => (<button onClick={() => history.push("/")}>home</button>)

Routes = withRouter(Routes);
```

### Additional props to Component rendered inside of Route

- if we need to pass more then just route props to our component, we can use `render` attribute of Route

```
let users = ["John", "Peter", "Bjorn"];

<Route path="/users/:name" render={props =>(
  <Users {...props} items={users} />
)} />


// we get props given by Router and items given by us
let Users = (props) => (<div> 
                          <h1> {JSON.stringify(props)} </h1>
                          <ul>
                            { props.items.map( (e, i) => (<li key={i}> {e} </li>) ) }
                          </ul>
                        </div>);

```

