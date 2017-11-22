# Many To One

One of the fundamental things to understand is that objects form graphs.
If we visually represent a shopping cart with it's content, we will see some structure that looks something like this:
```
     cart
   /   |   \
item  item  item
```
The cart above has 3 items, and we say that one cart has many items. Usually that means that a cart object has a collection that can contain 0, 1, 2, or more item objects.
This type of a relationship between objects is not easily translated into the database world, but it is possible.

As you know, it is customary to give tables the name of the class whose objects they store: Cart.java -> cart table, Item.java -> item table.

One of the possible, and most commonly used, ways to capture the fact that a cart has many items is to use foreign key in the item table.
Foreign key is a constraint that forces us to make sure that the value we store in one table, is present as an id value in some other table. What does it mean?

Let's say we have a table that has the following values as primary keys: 4, 5, 6.

```
Table person.
id | name
----------
4  | Bob
5  | John
6  | Bobert
```

In another table we can create foreign key constraint referencing the table above.

```
Table car.
id | person_id | make
----------------------
34 | 4         | Ford
35 | 5         | Toyota
36 | 6         | Smart
```

Our database will not let us insert a row into the car table that has a person_id value that is not an existing value in id column of a person table.

That is, our database guarantees that we will never have a car that doesn't belong to a person.

A natural extension of the idea above, is one person having more then one car, or the fact that a person can have no car at all.

```
Table person.
id | name
----------
4  | Bob
5  | John
6  | Bobert
```

```
Table car.
id | person_id | make
----------------------
34 | 5         | Ford
35 | 5         | Toyota
36 | 6         | Smart
```
What is happening here?
Bob doesn't have a car.
Lucky John has two cars, what make are they?
Bobert has one car.

If we focus on John, we can see why we can say that he has 2 cars: In the car table there are 2 rows that have his person_id.

Because the column called person_id is a part of the car table, we say that car owns the relationship. The information about the relationship is stored in the car table.

Let's bridge this knowledge with object oriented programming:

Visually:
```
     cart
   /   |   \
item  item  item
```
One cart has many items.

In Java:
```
public class Cart{
private Set<Item> items;
```
```
public class Item{
private Cart cart;
```

In Database:
```
Table cart
id | user_id | discount
-----------------------
5  | 767676  | 5
6  | 886677  | 0
```
```
Table item
id | cart_id | description
--------------------------
78 |    5    | towel
79 |    5    | plastic basket
80 |    6    | gnome
```

How many items are in each cart? How did you answer this question?

Just like you, our intermediary from the previous lesson, the one who seats between a program and a database, can answer these question.

It can get all of the items for a particular cart by doing the following:
1) take a cart's id
2) find all the rows in the item table that have this value as a cart_id.
3) create as many objects as the number of rows found and return them to the program.

Likewise, it can get us a cart that contains a given item:
1) take an item's id
2) lookup the item and see value stored in the cart_id column
3) go to the cart table and find row with this id.
4) populate a cart object with values from this row.

That is why we say a cart has one-to-many relationship with item and item has many-to-one relationship with cart.
In the same way, user has many cars, and a car has one owner.
