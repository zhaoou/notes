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
