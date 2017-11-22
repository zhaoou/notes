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
