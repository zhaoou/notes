Generics is compile time type safety mechanism in Java.
Type information is erased during compilation.

* Covariant - type of collection changes with the type of its content

`Object[] ray = new Integer[5];// Object[] is a supertype of Integer[]` 
* Invariant - type of a collection doesn't change with the type of its content

`List<Object> objects = new ArrayList<String>();// ERROR: List<Object> is NOT a supertype of ArrayList<String>`

**Java arrays are covariant and collections are invariant.**


# Covariance

Java arrays are covariant. Type of Integer[] is different from String[]. They are type checked at runtime and throw ArrayStoreException if we try to store wrong type.

```
Integer[] integers = new Integer[5];
Object[] objects = integers;
objects[0] = "string";      // Allowed at compile time, breaks during runtime.
```
> Arrays have run time type safety checks, unlike collections.


# Invariance

Given: 
```
List<String> strings = new ArrayList<>();
List<Object> objects = strings;
```
`List<Object> objects = strings;` is illegal, because that would make two references to the same collection, and we could put Objects using `objects` reference and try to get them as Strings via `string` reference. This would violate type safety.

> `if Foo extends Bar, G<Foo> is not a subtype of G<Bar>!`



# We can add exceptions to invariance in collections, to make our code more flexible, and keep type safety.

**we have two main mechanisms to reintroduce variance: type parameters and wildcards**

## Reintroducing variance: type parameters in generic methods

When we are calling a generic method, it will be parametarized by the type of arguments to the method.

We declare type parameter, like this`<T>` and use it later like this `T`.


```
          /-- generic parameter declared
         /
static <T> List<T> singleton(T item) {
                 \____________\
                               \-- generic parameter used
          
```

another example:

```
static <T> void fromArrayToCollection(T[] a, Collection<T> c) {
    for (T o : a) {
        c.add(o);
    }
}
```

Occasionally, JVM cannot infer type parameter, or we decide to be explicit, and we are allowed to pass type argument to a generic method like this:
```
Class.<Type>methodInvocation(Type a);
```


## Reintroducing variance: type parameters in generic classes


```
                          /-- type parameter
                         /
public class ArrayList<Item> implements List<Item> {
  public Item get(int i) { ... }
  public boolean add(Item x) { ... }
                       \
                        \-- use of the parameter, Item will be replaced by the actual type



                         /-- type parameters
                        / \
public interface Map<Key, Value> {
  Value get(Key x);
              \
               \-- use of the parameter, Key will be replaced by the actual type

```
When we are calling a constructor, type arguments are passed to the class:
```
new HashSet<String, Long>();
```
or inferred from reference variable:
```
List<String> strings = new ArrayList<>(); // diamond syntax
```


## Reintroducing variance: type bounds with wildcards

### Wildcard

Because of strict type safety rules, this code will accept List ONLY of Shapes, not any sub or superclass of Shape.

```
public void drawAll(List<Shape> shapes) {
    for (Shape s: shapes) {
        s.draw(this);
   }
}
```

But we often need to process a collection of things, like this:
```
void printCollection(Collection<?> c) {
    for (Object e : c) {
        System.out.println(e);
    }
}
```
Generic wildcard `?` allows us to pass Collection containing any types of Objects.

### Type bounds

**Extend to get, super to put**

We can use **upper bounded wildcards to read** elements from a collection:

```
public void drawAll(List<? extends Shape> shapes)
```

Wildcards are inclusive, i.e. this method can accept any List of Shapes, in addition to any List of any subtypes of Shape.

Examine:
```
class Collections {
    public static <T> void copy1(List<T> dest, List<? extends T> src) {}
    public static <T, S extends T> void copy2(List<T> dest, List<S> src) {}
}
```
Both methods achieve the same type constrain, but in copy2, S is only used for limiting type of src, hence copy1 is preferrable here.


We can use **lower bounded wildcard to add** elements to a collection:

```
static <T> void fill(List<? super T> list, T x) { ... }
```

This means that `list` can be a List<Q> as long as Q is higher in the heirarchy than T. Or, T must be a subclass of Q.
          
We will be using T as a reference to elements inside of this method, and can add T to the collection, which is OK, by substitution principle.
     
**When we are using wildcards for bounds, we are preserving type safety. Reading from a collection, we are using a supertype as a reference to each element. Writing to a collection, we are using a subtype of elements of that collection. This guarantees that all other references to this collection remain typesafe via Liskov substituion principle.**


          
https://www.youtube.com/watch?v=j82KHOL2FT8

