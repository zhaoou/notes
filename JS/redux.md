
### Redux

* Redux is an implementation of Flux pattern
* Redux manages state for the React app
* State is a tree like structure
* Some state is kept in react components

#### Redux Action

* A JS object representing one type of action, or event
* Keep payload small
* Action creator: small function creating action
* returns an object describing action and some payload

```
const mealCreator = id => ({ type: "CREATE_MEAL", id });
```



