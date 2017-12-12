```
  (---------root component----------)
  ( (state) (state update function) )
  (                                 )
       |               |
       |               |
   (prop 1)         (prop 2)     <- passed as props to child component
        \             /
         \           /      
       (---child component------------------)
       ( 1. renders state                   )
       ( 2. calls update function on action )
       ( 3. causes rerendering              ) 
```


## BASICS

### React element

- JS object, not a DOM node
- React.createElement(elementType, props, elementContent); // returns one root element
- props are properties to be applied to dome node(not actual element, see documentation) when rendering
- elementContent can be string, react element or react component or array of elements(array requires keys)
- ReactDOM can render this element in the browser
    ```ReactDOM.render(element, document.getElementById(“root”)); // root : <div id=“root”></div>```

### JSX

- alternative to .createElement()
- compiles into .createElement() calls
- syntax extension to JS
- write one html element in JS
- {evaluates everything here}

### React component

- reusable
- single responsibility
- render html, often using JSX
- extends React.Component
- still need to be rendered using ReactDOM.render
- maintains and manages **state**
```
class ContactList extends Component{
  state = { user: "John }; // in render method: {this.state.user} 
```
- state changes are reflected in the view **automatically**
- UI is a function of state
- react compares render() outputs before-after state change and updates DOM, aka reconciliation.
- using `setState` informs react of state updates
- v1: `setState({name: "Joe"});`
- v2: `setState( (oldState) => {count: oldState.count + 1} );`

### Composition

- use .createElement inside of another .createElement
- should return ONE element (with child elements)
- when rendering component, we can use other components in JSX
```
    class App extends Component {
    render() { return (  <div className="App">
                 	  		<ContactList/> // another component
      			        </div>v); }}
```
- we can also pass props to the component
- use props in the component to customize the component

```
class ContactList extends Component{
    render(){
        return <ol>
               {this.props.contacts.map( p => ( <li key={p.name}> {p.name}</li> ) ) }
               </ol>
    }
}

class App extends Component {
  render() {
    return ( <div className="App">
                 <ContactList contacts={[   {name: "john"},
                                            {name: "ron"},
                                            {name: "macaroon"} ]} />
                 <ContactList contacts={[   {name: "john"},
                                            {name: "ron"},
                                            {name: "macaroon"} ]} />
             </div>); } }
```


### State Management

**Props**
- state is passed to components that need it using props
- props are immutable
- props should not initialize state

**State**
- State resides in in the root Component
- Inside of the component holding state define a function responsible for state update
```
removeContact = (toRemove) => {
    this.setState(
      (s) => ({contacts : s.contacts.filter((c) => c.id != toRemove.id)}) )}
```
- Pass **state and state updating function(s)** to component(s) using props
```
<ListContacts onDeleteContact={this.removeContact} contacts={this.state.contacts}/>
```
- In that component, assign that function as a handler
```
onClick={() => props.onDeleteContact(c)} // c comes from the current iteration
```
- PropTypes: add type safety and props validation
```
import PropTypes from 'prop-types'; 
// ...component code... //
ListContacts.propTypes ={ 
    contacts: PropTypes.array.isRequired,
    onDeleteContact: PropTypes.func.isRequired
}
```

**Controlled components**

- **state resides in controlled component**
- components rendering a form, with form state stored in a component and not in DOM
- use if you want UI to update based on form value
- react controls the state of the form
```
class NameForm extends React.Component {
  state = {email: ""}
  handleChange = (event) => { this.setState({email: event.target.value}) } 
  render (){ 
    return( <form>
              <input type="text" value={this.state.email} onChange={this.handleChange} /> 
            </form> )}}
            
// any change in form value triggers handler which updates component's state, which triggers rendering by default.
```
- instant input validation
- conditionally enable/disable buttons
- enforce input formats

**Stateless Functional Component**
- components build by a single function
- `function User(props){ return (<p> props.username </p>); }`
- or `const Email = (props) => ( <div> {props.text} </div> );`
- not a class, don't uses `this` to get properties
- stateless


### Component lifecycle events:

- componentWillMount()
- componentDidMount() : good point for AJAX calls, setting state from this method will rerender UI.
- componentWillUnmount()
- componentWillReceiveProps()
- it is convinient to have all AJAX calling functions in one place

### Router
- creates links
- manages URLs
- BrowserRouter: listens to URL changes and renders the UI
- in index.js add `import { BrowserRouter } from 'react-router-dom'`
- in index.js wrapp app `<BrowserRouter><App /></BrowserRouter>` // enables URL listening
- `<Link/>`- clicking on a link tells BrowserRouter to update URL.
- `<Link to="/create"/>`
```
<Link to={{
  pathname: '/courses',
  search: '?sort=name',
  hash: '#the-hash',
  state: { fromDashboard: true }
}}> Courses </Link>
```
- Routers looks at URL and renders UI if url match is found
```
<Route exact path="/" render={ ()=> (<SomeComponent/>) } />
<Route path="/create" component={SomeComponent} />
```
- `history.push("/");` forces the app to go to the main route => router will render whatever component is mapped to `"/"`

### Getting Started
- create-react-app contacts
- cd contacts
- npm start

