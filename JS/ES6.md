## let and const

* let and const are block scoped

* not hoisted

* const is for constants, let is for variables

## template literals

* String in backquotes with `${value}`

```
`Hello, my name is ${user.name}`
```

## destructuring

* from object
```
const gemstone = {
  type: 'quartz',
  color: 'rose',
  karat: 21.29
};
const {type, color, karat} = gemstone;
console.log(type, color, karat);
```
* destructuring functions from objects looses `this` reference.

* from array
```
const point = [10, 25, -34];
const [x, y, z] = point;
```

## object literal shorthand

* works if object properties have the same name as variables

```
let type = 'quartz';
let color = 'rose';
let carat = 21.29;

const gemstone = {
  type,
  color,
  carat,
  calculateWorth: function() {} // same as below
  calculateWorth() {} // same as above! no need for function keywords
};
```

## for-of and iterable objects

* for-in iterates over all enumerable properties, even if they are from parent. Hence `hasOwnProperty` use.

* for-of only iterates over iterable types and only over enumerable fields.

* Object is not iterable

## spread & rest

* spread `...` spreads iterable objects into its elements

```
const fruits = ["apples", "bananas", "pears"];
const vegetables = ["corn", "potatoes", "carrots"];
const produce = [... fruits, ... vegetables];
// [ 'apples', 'bananas', 'pears', 'corn', 'potatoes', 'carrots' ]
```

* rest

```
const order = [20.17, 18.67, 1.50, "cheese", "eggs", "milk", "bread"];
const [total, subtotal, tax, ...items] = order;
console.log(items);// [ 'cheese', 'eggs', 'milk', 'bread' ]
```

* rest as a param to variadic function

```
function sum(...nums) {
  let total = 0;  
  for(const num of nums) {
    total += num;
  }
  return total;
}
```
## default parameters

* assign default value to parameter(s):
```
function say(word = "hi"){
  console.log( word + " world");
}
```

* with array destructuring:
```
function grid([w = 4, h = 5] =[]){ // each element and array itself is defaulted!
  console.log( w, " and ", h)
}
```

* with object destructuring:
```
function create({count = "5", color="red"} = {}){ console.log(count, color); }
```


## classes

* syntactic sugar on top of JS

```
class Person{
  constructor(name){ // constructor taking parameter
    this.name = name; // this is avilable
  }
  speak(phrase){ // method
    console.log( `${this.name} says ${phrase}`);
  }
  
  static jump(){ console.log("jumping"); } // static method

}

let jack = new Person("Jack");
```

* extends creates inheritance relationship

```
class Tree{
  constructor(name){ this.name = name; }
  describe(){ console.log(this.name); }
}

class Maple extends Tree{  // extends creates inheritance
  constructor(name, age){
    super(name); // must be first call in inheritance
    this.age = age;
  }
}

let tree = new Tree("poplar");
let maple1 = new Maple("maple1");
console.log(maple1 instanceof Tree); // -> true
```


## modules

* a providing module must export its public values (functions and variables)

```
// direct exports
export const message = "Hello"; 
export function sayHiToNinja() { return message + " " + ninja; }

// exporting with object literal shorthand
export { message, sayHiToNinja }; 

// aliasing export
export { sayHi as sayHello }; 
```

* a using module must import what it needs

```
// using destructuring
import { message, sayHiToNinja} from "Ninja.js"; 
console.log(message);

// using * notation and alias
import * as ninjaModule from "Ninja.js"; 
console.log(ninjaModule.message);
```

### default module exports & imports

* exports

```
// default export
export default class Ninja { 
  constructor(name){ this.name = name;}
} 

// additional exports possible with default
export function compareNinjas(ninja1, ninja2){ return ninja1.name === ninja2.name;}
```

* imports

```
// default imported and given a name
import ImportedNinja from "Ninja.js";

// import additional to default, with destructuring
import {compareNinjas} from "Ninja.js";
```

## promise

* promise is representation of asynchronous computation with composability built-in.

* promise takes two functions: one to be called on succes and another on failure.

```
var p = new Promise(function(resolve, reject) { // creation
  setTimeout(()=> {
    let even = Math.floor((Math.random() * 100) % 2) === 0; 
    if(even) { resolve(5); }
    else     { reject(new Error('promise failed')); }
  }, 2000);
	
});

p.then(function(r) { console.log('success', r); }) // use
.catch(function(e) { console.log(e); }) // 
```

* `Promise.all([])` takes an array promises and waits for them all to succeed before calling callback

* `Promise.all([])` resolves an array of results

```
Promise.all([promise1, promise2])
.then(function(results) {})
.catch(function(error) {});
```

* `Promise.race([])` works similar to `.all([])` but returns a result of the first promise to finish

* `Promise.resolve()` and `Promise.reject()` immediately return promise


```
var ps = Promise.resolve(5);
var pf = Promise.reject(new Error('promise failed'));
```
