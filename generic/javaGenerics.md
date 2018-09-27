# type parameters vs. type argument


# generic subtyping

Given: 
```
List<String> strings = new ArrayList<>();
List<Object> objects = strings;
```
`List<Object> objects = strings;` is illegal, because that would make two references to the same collection, and we could put Objects using `objects` reference and try to get them as Strings via `string` reference. This would violate type safety.

> `if Foo extends Bar, G<Foo> is not a subtype of G<Bar>!`

# generic methods

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

# generic classes and interfaces

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
# bounds

Because of strict type safety rules, this code will accept List ONLY of Shapes, not any subclass or superclass of Shape.

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


# complex bounds

If a method, by design, can only work **at or below a certain type(class or interface) level**, we can use upper bounded wildcard:

```
public void drawAll(List<? extends Shape> shapes)
```

Upper bounded wildcards are inclusive, this method can accept any List of Shapes, in addition to any List of any subtypes of Shape.


# erasure
