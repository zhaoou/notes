# Streams in Java 8

## Lambdas

In Java, lambdas can only be used as an instance of a Functional interface.

There are 6 important functional interfaces in Java:

#### 1) Predicate\<T>- takes an instance of T and returns boolean

Predicates can be combined by using `.and(Predicate<T> p1).or(Predicate<T> p2).negate()` methods.

#### 2) Consumer\<T>- takes an instance of T and returns nothing

Can be combined by using `.andThen(Consumer<T> c)`.

#### 3) Supplier\<T>- takes nothing and returns an instance of T

#### 4) Function\<T,R>- takes instance of T and returns an instance of R, which may or may not be the same type as T

Can be combined: `.andThen(Function<R,Q> func)`

#### 5) UnaryOperator\<T>- takes one instance of T and returns one instance of T

#### 6) BinaryOperator\<T>- takes two instances of T and returns one


### Lambda variables

Inside of the lambda expression:

* We **can use effectively final local variables**.

* We **can modify instance variables**.

**Free vs. Bound variables aside: `f(x) = x + y`, `x` is bound(to argument x) and `y` is free.**
So in lambda, free variables are saved for future time, aka closure.

## Using lambdas in streams

Streams are cool.
This new feature allows us to use lambdas to operate on data, similar to functional composition in functional programming. I have been using them for several months now, and learned to love it. Combine streams, spring data, spring boot and you can enjoy development again!

I noticed that readability and size of the code improved significantly, since I started using streams. There are some instances where it is not easy to rewrite older java code using streams, but generally it's been great!

So what is a stream? Stream is a structure that allows one to create an expression that can be evaluated.

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

If you want a single result, we can use reduce function, or use IntSummaryStatistics class.

Reduce takes an identity value and a function used to combine intermediate result of reduce and and element of the stream:

    .stream().reduce(0, (x, y) -> x + y);

IntSummaryStatistics calculates average, max, min and sum.

    .stream().collect(
        Collectors.summarizingInt(String::length)
    );

Returns an instance of IntSummaryStatistics containing the statistics.

LongSummaryStatistics and DoubleSummaryStatistics are also provided.

As always, this is just a high level overview that helps me reason about streams.
