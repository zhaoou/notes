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
}

let jack = new Person("Jack");
```
