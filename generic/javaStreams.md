# Streams and lambdas

## Lambdas

**Lambdas define code to be used later.**

In Java, lambdas can only be used as an instance of a Functional interface. Functional interface is any interface that has only one abstract method. (SAM - single abstract method). Lambda expressions will be replaced by an object implementing that interface, with a method generated from the lambda expression.

Additonally, there are 6 important functional interfaces added to Java:

#### 1) Predicate\<T>- takes an instance of T and returns boolean

Predicates can be combined by using `.and(Predicate<T> p1).or(Predicate<T> p2).negate()` methods.

#### 2) Consumer\<T>- takes an instance of T and returns nothing

Can be combined by using `.andThen(Consumer<T> c)`.

#### 3) Supplier\<T>- takes nothing and returns an instance of T

#### 4) Function\<T,R>- takes instance of T and returns an instance of R, which may or may not be the same type as T

Can be combined: `.andThen(Function<R,Q> func)`

#### 5) UnaryOperator\<T>- takes one instance of T and returns one instance of T

#### 6) BinaryOperator\<T>- takes two instances of T and returns one

Broadly speaking, we can classify most interfaces into 6 categories above.


### Lambdas and variables

1) All lambdas are defined in some method, i.e. a method is called, and inside of it we create a lambda expression.

2) Each new method call creates a new stackframe on a callstack.

3) When method is done, it is removed and all local variables are lost.

4) Free vs. Bound variables review: `f(x) = x + y`, `x` is bound(to argument x) and `y` is free. Free variable is usually defined outside of the scope of the function. For example in the stackframe defining the lambda.

5) Lambdas are implemented as closures. A closure is an object, implementing one interface, has one method, and contains copies of all free variables, as they were available in the stackframe when that lambda was defined.

6) To makes sure that copies of variable placed in a closure are correct, compiler forces us NOT to change those free variables after the lambda is defined. Only final or effectively final free variables can be used in lambdas.

7) If we are using object reference variable in a lambda, it should be effectively final. We still can change the instance variables of that object. If we change object variables in a multithreaded environment, we can create problems if we don't use synchronisation.

http://www.informit.com/articles/article.aspx?p=2303960&seqNum=7



## Lambdas and streams

This new feature allows us to use lambdas to operate on data, similar to functional composition in functional programming.

So what is a stream? Stream is a lazy structure that allows us to create an expression that can be evaluated.

There are **3 main** parts to any stream:

* beginning
* middle
* end

**Beginning** of the stream is simple a source of data, it can be a collection, or a function generating infinite stream. All collections now have ``.stream()`` method that returns a stream of whatever is in that collection. For arrays we can use ``Stream.of(array);`` method, or if we need an empty stream, we can use ``Stream.empty();`` to obtain that. The coolest addition, is infinite streams: provide initial value and a function that will be used to generate all consecutive values.

    Stream.iterate(BigInteger.ZERO, n -> n.add(BigInteger.ONE));

The coolest thing about this structure is that previous values is remembered and we don't have to manage state at all.

**Middle** of the stream is where all of the magic happens.
Here we can use map function:

    words.stream().map(String::toLowerCase);

This line will return a stream of words with the function applied to them. It will not modify the source: words.

We can also use filter:

    words.stream().filter(w -> w.length() < 8);

This will return a stream of words that are 7 characters or less in length.

We can also *chain* the intermediate operations on streams:

    words.stream().map(String::toLowerCase).filter(w -> w.length() < 8);

All other goodies from FP are here:

    words.stream().limit(5); // take 5
    words.stream().skip(5); // drop 5
    words.stream().distinct(); // uses equals() to find duplicates

    words.stream().filter(s -> s.startsWith("A")).findFirst();        //first word that begins with 'A'
    words.stream().filter(s -> s.startsWith("A")).findAny();
    // any word that begins with 'A'.

A slightly more complex operation is `flatMap()`. It does 2 things: 

* maps a function that returns a stream (e -> stream) over each element of the original stream, and 
* flattens the resulting stream of streams into a simple stream.

```
Stream.of("a b c", "d e f")
.flatMap( a -> Stream.of(a.split("\\s")) )
.forEach(System.out::println);
```

We can also sort the elements of a stream:

    words.stream().sorted(
        Comparator.comparing(String::length).reversed());

In this example strings are sorted by length in descending order. This feels a lot like we are telling what, not how, should something happen.

**Ending** the stream is a bit different, we are trying to use all of the data available after all transformation and filtering is done. We have to decide if we want a collection of the elements or a single result from that collection. If we want a collection, we just need to append

    .collect(Collectors.toList());
    .collect(Collectors.toSet());
    .collect(Collectors.toMap(User::getId, User::getName));
    // key, value.
at the end of the stream.

If you want a single result, we can use reduce function, or use IntSummaryStatistics (or similar) class.

Reduce takes an identity value and a function used to combine intermediate result of reduce and and element of the stream:

    .stream().reduce(0, (x, y) -> x + y);

IntSummaryStatistics calculates average, max, min and sum.

    .stream().collect(
        Collectors.summarizingInt(String::length)
    );

Returns an instance of IntSummaryStatistics containing the statistics.

LongSummaryStatistics and DoubleSummaryStatistics are also provided.
