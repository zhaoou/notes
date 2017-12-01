#### BASICS

React element 
- JS object, not a DOM node
- React.createElement(elementType, props, elementContent); // returns one root element
- props are properties to be applied to dome node(not actual element, see documentation) when rendering
- elementContent can be string, react element or react component or array of elements(array requires keys)
- ReactDOM can render this element in the browser
    ```ReactDOM.render(element, document.getElementById(“root”)); // root : <div id=“root”></div>```

JSX
- alternative to .createElement()
- compiles into .createElement() calls
- syntax extension to JS
- write one html element in JS
- {evaluates everything here}

Nesting
- use .createElement inside of another .createElement
- use JSX 
- both should return ONE element (with child elements)

React component
- reusable
- single responsibility
- render html, often using JSX
- extends React.Component
- still need to be rendered using ReactDOM.render

Composition
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



Getting Started
- create-react-app contacts
- cd contacts
- npm start


#### STATE

- state resides in in the root Component
- state is passed to components that need it using props


##### Props - Stateless Functional Components - Controlled Components

Props
- pass data into components

Stateless Functional Component
- components build by a single function
- `function User(props){ return (<p> props.username </p>); }`
- `const Email = (props) => ( <div> {props.text} </div> );`
- not a class, don't uses `this` to get properties

Controlled components
- form hooked up to state
