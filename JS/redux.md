
### Redux

* Redux is an implementation of Flux pattern
* Redux manages state for the React app
* Redux consists of **store, actions and reducers**
* State is a tree like structure
* Some state is kept in react components

#### Action

* A JS object representing one type of action, or event
* Keep payload small
* Action creator: small function creating action
* returns an object describing action and some payload

```
const mealCreator = id => ({ type: "CREATE_MEAL", id });
```

#### Reducer

* A JS function that takes state and action and returns new state

```
function appReducer(state, action){
    switch(action.type){
        case "DELETE_FLAVOR":
            return state.filter((e) => e.flavor !== action.flavor);
        default:
            return state; } }
```


#### Store

* Holds the application state
* `Redux.createStore(reducer);` 
* Passed to the app as a prop
* In componentDidMount we connect store's state with the component state by subscribing
* `.subscribe(listener);`
* `.getState();`
* we also connect component's events with the store
* `.dispatch(action);` finds and invokes the right reducer


#### 
```

|-Component-|                                             |-Store----------|
|           |----(dispatches Action)--------------------->|                |
|           |                                             |->calls reducer |  
|           |                                             |->gets new state|
|           |<---(mapStateToProps called with new state)--|                |
|-----------|                                             |----------------|

```

### React-Redux

#### Provider

* uses React's context to pass data from the store to any component that needs it.

#### connect()

* `connect(mapStateToProps, mapDispatchToProps)(MyComponent)`
* connects a React component to the Redux store
* `mapStateToProps()` allows us to specify which state from the store you want passed to your React component
* `mapDispatchToProps()` allows us to bind dispatch to your action creators before they ever hit your component

##### `MyComponent` 

* Will recieve store's state and/or dispatch function reference

##### `mapStateToProps()`

* function (current store, current props) =>  new props for MyComponent
* `mapStateToProps(storeState, [ownProps])`
* component will subscribe to Redux store updates 
* if the store is updated, mapStateToProps will be called.
* must return a plain object, which will be merged into the component’s props
* just a function that lets connect() know how to map specific parts of the store’s state into usable props

##### `mapDispatchToProps()`

* allows to bind `dispatch()` to action creators
