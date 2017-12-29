
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
* we also connect component's events with with the store
* `.dispatch(action);`


