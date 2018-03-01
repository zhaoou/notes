## let and const
## template literals
## destructuring
### arrays
### objects
## object literal shorthand
## for-of and iterable objects

* for-in iterates over all enumerable properties, even if they are from parent. Hence `hasOwnProperty` use.

* for-of only iterates over iterable types and only over enumerable fields.

* Object is not iterable

## spread & rest

* spread `...` spread iterable objects into elements

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

### rest as a param to variadic function

```
function sum(...nums) {
  let total = 0;  
  for(const num of nums) {
    total += num;
  }
  return total;
}
```
