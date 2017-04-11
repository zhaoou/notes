# Ocaml intro

As I was finishing my midterm yesterday, I realized that professor Shieber did a very good job teaching us foundations of FP.

Since we have already covered the basics, I feel like it is the right time for me to write about them.

**Idea #1**

Higher order functions, are the biggest attraction of FP.
The fact that we can compose functions in various ways, partially apply them, use them as arguments to other functions and return them from functions allows us to achieve another level of code reuse: functional composition.

**Idea #2**

Since most structures are functions, the best way to control code is recursion. There are two main types of recursive functions, tail recursive and not tail recursive. If the languages runtime allows, tail recursive functions reuse stack frame, effectively converting recursion to iteration. To achieve that, a function should call itself, but also this should be the last thing this function does. If it is expecting a return value to finish the computation, or modifies the result before returning it in any way, it is not tail recursive. It will hold on to the stack frame until all recursive calls return. Sometimes, it is possible to convert a not tail recursive function into a tail recursive function by adding another parameter, accumulator. Accumulator represents the result of the computation so far. As it turns out, this is a very useful pattern to use, specially in higher order functions like fold.

**Map**

Map is one of those functions, essential to understanding of the FP.
Given a list, we need to do something with this list. This often means doing something on each element of the list, and using the results of each computation. This is exactly what map does, with the added bonus of FP, you can define what needs to happen with each element, inside of a function.

In concrete terms, map takes a list and a function that takes an element of a list, computes something, and returns a result of that computation. The return of a map is a list of results of computations obtained by applying map to elements of the argument list.

    let rec map (f: 'a -> 'b) (l: 'a list): 'b list =
      match l with
      [] -> []
      | h::t -> (f h) :: (map f t)

Many things are happening here, but we can see that we take a function from 'a to 'b, a list of 'a, and return a list of 'b. The argument list can either be an empty list, or a list that has a head and a tail. If it is an empty list, the result of map is an empty list. If it has a head, we apply our function to the head element, and cons (join) the result with the result of applying map to the rest of the list, which we obtain by calling `map` recursively. We are guaranteed to reach base case because the size of the problem is reduced with each recursive call.


**Filter**

Filter is very similar to a map, but the function it takes, decides whether the element is included in the returned list. It allows us to filter elements of a list based on some test, that we provide as a functional argument to filter.

    let rec filter (f: 'a -> 'b) (l: 'a list) : 'b list =
      match l with
      [] -> []
      |h::t -> if f h then h :: filter f t
                      else filter f t

Main logic is happening when we have a non-empty list and test if the function `f` applied to the head `h` returns true, if it does this element `h` is included in the output. We obtain the rest of the filtered list by calling filter recursively on the rest (tail `t`) of the list. Recursion will terminate once we reach an empty list.



**fold\_left and fold\_right**

These are the functions that encapsulate an idea of iteration over a list and computing some value as we go
These functions take an accumulator, a list and a function that knows how to take an element of a list, combine it somehow with the accumulator and returns the resulting accumulator. Good news is that accumulator can be anything: a number, another list, or an empty list.

    let rec fold_left f a l =
      match l with
      [] -> a
      |h::t -> let result = (f h) in
               fold_left f result t
and

    let rec fold_right f l a =
      match l with
      [] -> a
      |h::t -> let result = (fold_right f t a) in
               f h result

The biggest differences between fold left and right are:

* fold\_left processes the list from left to right and fold\_right from right to left.
* fold\_left is tail recursive, the last thing it does is calling itself, and current stack frame can be released or reused.
* fold\_right is not tail recursive, and it maintains the stack frame waiting for other invocations to complete, once the result from recursive call is obtained, it is used to complete the calculation in `f h result` part.

Using these 4 functions is a great start for anybody interested in solving problems faster!
