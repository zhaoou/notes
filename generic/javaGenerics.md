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

# generic classes and interfaces

```
                         |<--- type parameter
                         |
public class ArrayList<Item> implements List<Item> {
  public Item get(int i) { ... }
  public boolean add(Item x) { ... }
                       \
                        |<-- use of the parameter



                         |<--- type parameters
                        / \
public interface Map<Key, Value> {
  Value get(Key x);
              \
               |<-- use of the parameter

```
# bounds
# complex bounds
# erasure
