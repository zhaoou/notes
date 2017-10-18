# AngularJS notes


* In JS, we have scopes {{}}, so that 2 objects inside another object have separate scopes.
* HTML supports non-standard attributes using 'data-*', like 'ng-*'.

### Angular connects HTML and JS.

#### To do that we need to connect V and C on high level, using angular app

In JS: create an app in global scope
In HTML: connect app with html using `ng-app`
Angular does: finds an app in JS and `ng-app` in html and connects them.

#### We can now add low level functionality to this basic setup: html (view) and JS object (angular app).

In JS: add controller object to the app and give it a name
HTML: inside of the `ng-app` element add and element and bind it to controller using `ng-controller`.
Angular does: associates this controller with this element

#### Angular uses scope object to bind controller and view

* `$scope` is an angular object injected for us
* we can add objects/functions to it
* binds controller and view

How does it work?
* JS functions converted to string
* angular injector parses the string signature for parameter list
* injects parameters, like `$scope` into function.







