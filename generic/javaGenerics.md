Generics is compile time type safety mechanism in Java.
Type information is erased during compilation.

* Covariant - type of collection changes with the type of its content
* Invariant - type of a collection doesn't change with the type of its content

**Java arrays are covariant and collections are invariant.**


# Covariance

Java arrays are covariant. Type of Integer[] is different from String[]. They are type checked at runtime and throw ArrayStoreException if we try to store wrong type.

> Arrays have run time type safety checks, unlike collections.


# Invariance

Given: 
```
List<String> strings = new ArrayList<>();
List<Object> objects = strings;
```
`List<Object> objects = strings;` is illegal, because that would make two references to the same collection, and we could put Objects using `objects` reference and try to get them as Strings via `string` reference. This would violate type safety.


> `if Foo extends Bar, G<Foo> is not a subtype of G<Bar>!`
> We can add exceptions to invariance in collections.


# Reintroducing variance: generic methods

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

When we are calling this method, it will be parametarized by the type of arguments to the method: array and collection.

# Reintroducing variance: type parameters

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
> Use type parameters when there is a relationship between types we need to worry about

# Reintroducing variance: type bounds

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
Generic wildcard `?` allows us to pass Collection containing any types

Passing a collection doesn't allow us to modify it, since we don't know the real type of the objects in the collection, and can potentially violate type safety by addding objects of incorrect type.

> Every time we are using wildcards, we lose ability to modify that collection

> Use bounds where polimorphic behavior is needed


# Reintroducing variance: complex type bounds

** Extend to get, Super to put **

If a method, by design, can only work **at or below a certain type(class or interface) level**, we can use upper bounded wildcard:

```
public void drawAll(List<? extends Shape> shapes)
```

Upper bounded wildcards are inclusive, this method can accept any List of Shapes, in addition to any List of any subtypes of Shape.

Examine:
```
class Collections {
    public static <T> void copy1(List<T> dest, List<? extends T> src) {}
    public static <T, S extends T> void copy2(List<T> dest, List<S> src) {}
}
```
Both methods achieve the same type constrain, but in copy2, S is only used for limiting type of src, hence copy1 is preferrable here.


If a method, by design, can only work **at or above a certain type level**, we can use lower bounded wildcard:

```
static <T> void fill(List<? super T> L, T x) { ... }
```
This means that L can be a List<Q> as long as Q is higher in the heirachy than T. T must be a subclass of Q.
     
     
          
https://www.youtube.com/watch?v=j82KHOL2FT8

