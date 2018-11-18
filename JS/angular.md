## Template Driven forms

### NgForm
* represents entire form
* creates FormGroup that stores form data
* finds all elements marked with NgModel and adds them to formGroup
* can be bound to a local variable during development:
```
<form #f="ngForm"></form>
<pre>{{ f.value | json }}</pre>
```
* intercepts submit event and replaces it with ngSubmit event


### NgModel
* marks a form element as belonging to the form data
* stores value of the form field with the key equal to name form property:
```
<input type="text" name="username" ngModel>
```

### NgModel
* groups form fields using FormGroup nested object:
```
<form #f="ngForm">
  <div ngModelGroup="passwords">
    <input type="text" name="password" ngModel>
    <input type="text" name="pconfirm" ngModel>
  </div>
</form>
```

## Component communication: inputs and outputs

### Inputs
A component can receive inputs from outer component, by marking field(s) or setter(s) with `@Input()`.
An outer component has to provide the value for the child component:

```
<order-processor [stockSymbol]="stock" ...
// stockSymbol is the property name or setter in the child component
// stock is the variable in the parent component
```

### Outputs
Likewise, parent component can receive events from inner component by defining an even handler

```
<price-quoter (lastPrice)="priceQuoteHandler($event)"></price-quoter>
```

Child component must provide EventEmitter and mark it with `@Output()`

* EventEmitter is a generic type and accepts `Type` argument for the type of the event it emits.
* EventEmitter has an `emit(Type event)` method that is called every time an event occurs.
