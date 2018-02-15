### Basic data types

* null - is a special location on the heap, variable is assigned this special value.

* undefined - a variable that is declared but not assigned anything, even null.

* NaN - not a number, special value representing impossible result.

* Numbers: ```1, 3.0, 5.55```.

```5.66666.toFixed(2); -> "5.67"``` // returns a string of a number rounded up with the precision of 2.

* Strings: '1', "abc", "".

```"3" + 5 * 10; -> "350"``` // can concatenate strings

```"abc"[1]; -> "b"``` // can access like array, by index

```"abc\"d\"efg"``` // can use escape characters: ```\\, \", \', \n, \t```.

* JS is loosely typed: we don't specify type, JS engine performs type coercion.

* `==` and `!=` convert to the same type and then compares, often incorrect to use.

* `===` and `!===` checks if both are the same type and value, often what we need.

* Conditionals

``` 
if(true){} 
else if(false){} 
else{}
```

* Falsy values: converted to false in boolean context

> ```false, null, undefined, 0, "", NaN. ```

* Loops:

```
// while:
var i = 0;
while(i < 10) { console.log(i++); }

// for:
for (var j = 0; j < 5; j++) { console.log(j); }
```

* Functions:
```
function(){} -> undefined // by default returns undefined

function(name){ return name; } // identity function

var identity = function identify(name){ return name; } // storing function in a variable

identity(true); -> true
identity(5); -> 5

identify(false); -> ReferenceError // once stored in a variable, we cannot call named function directly

```

* Scope:

1) Global - var defined in global scope is accessible everywhere

2) Function - var defined in a function is accessible in that function only

Interpreter searches up the scope tree until it finds a variable or fails to find it and throws an exception:
```
{ 1
  { 2 }
  { 3 }
}
```
From block 2 we can see variables defined in 2 and 1, but not 3.

Variables defined in both global and function scopes: function scoped variable will 'shadow' the globally defined variable.

* Arrays
```
var numbers = [1, 2, 3];

numbers.length -> 3
numbers.push(9) -> [1, 2, 3, 9];
numbers.pop() -> 9 
numbers.splice(1, 1, 7, 8) -> [1, 7, 8, 3] // from index 1, remove 1 element, and insert 7 and 8
numbers.splice(-2, 0, 55, 66) -> [1, 7, 55, 66, 8, 3] // after second element from the end, remove 0 elements, insert 55, 66
numbers.forEach((element, index, array) => { array[index] += 100; } ); // add 100 to each element in the array
numbers.forEach((el, i, arr) => { arr[i] += el;}); // double each element

var mixed = [1, "a", null, [] ];
mixed[0] -> 1
mixed[3] -> []
```

* Objects
```
var shoes = {
  color: "brown",
  size: 13,
  lace: ["left", "right"],
  changeColor: function(newColor){
    shoes.color = newColor;
  }
};

shoes.lace -> ["left", "right"] // dot notation to access property
shoes["color"] -> "brown" // array notation to access property
shoes.changeColor("red"); // method changes color to red
```

* typeof

> returns a type of the variable
``` typeof false -> boolean```

* `this` can refers to different things depending on situation.

global scope -> window object

on an object -> the object on which it is called

in a constructor -> on the newly created object

when explicitly setting `this` using `bind, apply, or call` -> this will refer to that object

*  bind vs. apply vs. call

bind - permanently binds returned function to an object: ```var boundFunction = logThis.bind({name: "Stepan"});```

apply - invokes using object reference and an optional array of arguments: ```logThis.apply({name:"hi"}, []);```

call - invokes using object reference and optional arguments to the function being called: ```logThis.call({name:"hi"});```

### foreach, map, filter, reduce

foreach **takes element, index and array and returnes undefined**, designed for side effects, not building new array, unlike

map takes a function that **takes element, index and array and returns a new element**.

filter takes a function that **takes element, index and array and returns true / false**.

reduce takes a function that **takes accumulator, element and returns new accumulator value**.

### Constructor

a function that is used to create new instances of itself

when used with `new` is given a new empty object, and implicit return statement

has a prototype property that will become a prototype of all instances created by that constructor

we can add methods to all instances by adding them to constructor's prototype:

```
Rabbit.prototype.speak = function(line) {
  console.log("The " + this.type + " rabbit says '" + line + "'"); };
```

sometimes (if we need a map) we create prototypeless objects: `var map = Object.create(null);`

### Prototype

prototypes are used for property lookup if not found on the object

property can be `enumerable` and `nonenumerable`

enumerables show up in for-in loops:

```
for(var p in user){}
```

properties created by simple assignment are enumerable, adding nonenumerables looks like this:
```
Object.defineProperty(Object.prototype, "hiddenNonsense",
                      {enumerable: false, value: "hi"});
```

all enumerable properties of all prototypes show up in for-in loops, we can check if property belongs only to the object:
```
user.hasOwnProperty("age");
```
#### Setters and getters

we can define properties via setters and getters:

```
var pile = {
  elements: ["eggshell", "orange peel", "worm"],
  get height() {
    return this.elements.length;
  },
  set height(value) {
    console.log("Ignoring attempt to set height to", value);
  }
};
```

height is a virtual property.

We can add setter/getter to a prototype:
```
Object.defineProperty(TextCell.prototype, "heightProp", {
  get: function() { return this.text.length; }
});
```

### Spread and rest

Given array `ray`, `...ray` evaluates to the elements of the array, aka **spread**.

```
let ray = [3, 4];
let ray2 = [1, 2,  ...ray,  5];
```

Given a variable length list of elements,  `...elements` evaluates as array from those elements, aka **rest**.

```
function doThat(...things){ 
// 1. has things array block scoped variable
// 2. must be last in the parameter list
}
```

