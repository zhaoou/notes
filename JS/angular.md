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
