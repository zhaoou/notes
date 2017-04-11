# Spring JPA

#JPA is a high level persistence abstraction.

JPA allows us to have transparent persistence: moving objects from heap to permanent storage and back, without any SQL.

Set of all objects living in one database is called persistence unit. EntityManager(EM) is what allows us to CRUD objects in that DB, **EM represents one database**. Not all objects have persistent representation, and the once that do, are called **entities**.

An entity can be in 4 states: **new-managed-detached-removed**

When a **new** object is created, it lives on the heap, and has no database presentation(no row in the database).

EntityManager.persist() is used to move this new object to **managed** state. This means that EM will keep track of all the changes to it, for it is in the name: entity manager. When we save/flush that entity, the changes made to it will be reflected in the database.

If we load an entity and **detach** it, then it will no longer be managed by the EM, and instead will just be an object on the heap. Changes to this object, will not be reflected in the database next time we synchronize managed entities with the DB.

Say we loaded the entity into memory from the DB, and decided we don't need it in the database, but could still use it in memory. In this case we call **detach**(), this will delete DB entry representing this entity.

**EM allows us to move objects between DB and heap.**

Try and draw a diagram where you have two areas: heap and DB, and visualize all of the operations described above. Where does entity manager fit in?

Now let's take a closer look at entities and their relationships.

JPA allows us to embed an object in an entity:
An object that is one-to-one with the entity, and that doesn't exist without its owning entity, can be annotated with **@Embeddable**. In the database, it will be stored in extra columns in the same table as entity.

An entity can also have a collection of @Embeddable(s) or basic types like String. Use **@ElementCollection** to transparently store these collections. It defaults to lazy fetch, and is completely dependent on the lifecycle of the owning entity.

```
@ElementCollection
private List<Phone> phones;

@ElementCollection
private List<String> tags;
```

Next, we look at aspects of storing entity relationships:

* unidirectional / bidirectional relations
* @OneToMany, @ManyToOne, @OneToOne, @ManyToMany

##uni/bi-directional relations

**Unidirectional** relations allow one entity to refer to another. That is, entity navigation is possible in one direction only: from Item to Order.

**Bidirectional** relations allow us to navigate in both directions: Item to Order and Order to Item.


Directionality of the relationships between entities only affects the way they are represented in Java. In a DB, unidirectional and bidirectional relationships are represented in the same way.

Another **difference between uni/bidirectional** relationships is that **in a unidirectional** relationships:

 * Owner of the relation is the entity with @\*To\* annotaiton
 * After linking, save save the owning entity.

And **in a bidirectional** relationships:

 * You are telling the ORM that these two unidirectional relatioships are in fact one bidirectional relationship, by using `mappedBy` on the inverse (non-owning) side.
 * After bidirectional linking, save the owning entity to save new relation.


##@OneToMany, @ManyToOne
(fyi: examples inspired by the Hibernate in Action book)

There are:

* unidirectional @OneToMany
* unidirectional @ManyToOne
* bidirectional @OneToMany (@ManyToOne on the owning side).

relations.

#####Unidirectional @OneToMany:

```
@Entity
public class Item {
 @OneToMany
 @JoinColumn(nullable = false)
 public List<Bid> bids = new ArrayList<>();
// Bid doesn't have @ManyToOne and reference to Item
```

#####Unidirectional @ManyToOne

```
@Entity
public class Bid {
 @ManyToOne(fetch = FetchType.LAZY)
 @JoinColumn(nullable = false)
 private Item item;
}
```

#####Bidirectional @OneToMany - @ManyToOne
are different sides of the same relationship. One entity is the *many* side, and the other is the *one* side.

In bidirectional relationships, we have to identify two sides: owning side and inverse side. In @ManyToOne, **many side always owns the relationships**.

```
// Inverse side:
public class User{
 @OneToMany(mappedBy="user")
 private Set<Roles> roles = new HashSet<>();


// Owning side:
public class Role{
 @ManyToOne
 private User user;

```
When adding a new Role to a User, we have to make sure we double link the references!!!




##@OneToOne, @ManyToMany

#####@OneToOne

There are:

* unidirectional @OneToOne
* bidirectional @OneToOne

relationships.

We can think of @OneToOne as a case of @OneToMany, where many side can only have one entity.

**Unidirectional @OneToOne** are similar to unidirectional @OneToMany, scroll up!

**Bidirectional @OneToOne:**
Which side owns the relationship is decided base on use case. We should identify owning side, in the inverse side class, with *mappedBy* notation.

Here, one user has one role:
```
// Inverse side:
public class User{
 @OneToOne(mappedBy="user")
 private Roles role;

// Owning side:
public class Role{
 @OneToOne
 private User user;
```


#####@ManyToMany

The most important thing about @ManyToMany is that we need to use a join table. This type of relationships is not frequently occuring, and not very complex to implement.
We can choose which side owns the relationship, depending on the use case.


Further reading:
https://docs.oracle.com/cd/E19798-01/821-1841/6nmq2cpai/index.html
https://en.wikibooks.org/wiki/Java_Persistence
