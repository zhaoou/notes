# Joins in SQL

SQL, as we know, is a funny language. I think that the two most important aspect of SQL are transactions and joins. Joins are things that supposed to help us simplify queries over several tables, but based on the number of questions on stackoverflow, they actually confuse developers more than anything else.

Having been struggling with joins myself for years, I decided to end this struggle the only way I know how: learning the sh*t out of it.

Having read a few posts like this [one](http://blog.codinghorror.com/a-visual-explanation-of-sql-joins/), I feel that set based visual explanation is nice, but doesn't really help with the real life situations I encounter.

####Joins add columns to the result set based on a certain condition.

I found that in practice, I almost always use inner join, aka join. Inner join is similar to an intersection in set theory, elements that are present in both A and B. Taking it one step further inner join of A, B, and C, consists of elements that are elements in A, and in B and in C.

Given sets
A{ 1,2,3,4 },
B{ 3,4,5,6 },
C{ 6,7,3,2 }

Inner join of A and B is {3,4},
Inner join of B and C is {3,6},
Inner join of A and C is {2,3}.

Also, inner join of A and B and C is {3}, because 3 is the only element present in all three sets.

When working with sets of integers, it is very easy to see what is what, but real databases contain tables and column names. That complicates our reasoning about joins a little bit. When performing joins it helps to think of tables as namespaces and column names as members of that namespace.

When we join rows from one table with the rows in another table we get a row that contains the columns from both of these tables. When we join 3 tables we get a table that contains columns from all 3 tables. Joining the 3rd and nth table is actually joining the result of the previous joins with the nth table.

Strictly speaking, unless we join on the same field in all tables, this is not an intersection from the set theory. In practice, we need decide field on which to join the tables, and it may or may not be the same between all tables.

As the number of tables we join goes up, the likelihood of namespace conflicts goes up as well. As tables define a namespace, there is a chance that column names in two tables are the same.

Time for an example:

This (silly) SQL:

    CREATE TABLE Person(    id int,
                            name varchar(255),
                            city varchar(255));
    create table Purchase(  person_id int,
                            item_id int,
                            item_count int);
    create table Item(      id int,
                            price int);

    insert into Person values( 1, "John", "lucerne");
    insert into Person values( 2, "Adam", "wilton");
    insert into Person values( 3, "Dick", "torono");
    insert into Purchase values( 1, 2, 300);
    insert into Purchase values( 3, 1, 50);
    insert into Purchase values( 2, 3, 200);
    insert into Purchase values( 1, 2, 70);
    insert into Item values ( 1, 70000);
    insert into Item values ( 2, 55000);
    insert into Item values ( 3, 22000);

creates these tables:

Person
1 John lucerne
2 Adam wilton
3 Dick torono

Purchase
1 2 300
1 2 70
2 3 200
3 1 50

Item
1 70000
2 55000
3 22000

Now if we run `select * from Person;` we get

1 John lucerne
2 Adam wilton
3 Dick torono

where all of the rows come from Person, since we are not doing the join yet. We can build on that result by using joins.

Let's add some information from Purchase:

    select * from (
     Person pn
     join
     Purchase ps
     on pn.id = ps.person_id
    );

We should get something like this:

1 John lucerne 1 2 300
2 Adam wilton  2 3 200
3 Dick torono  3 1 50
1 John lucerne 1 2 70

Where left half of the result is from the Person and the right is from the Purchase. The resulting set can contain 0 or more tuples. It will contain 0 rows if none of the rows in the original tables meet the condition we defined in `on pn.id = ps.person_id` clause.

We can take it even further:

    select * from(
     Person pn
      join
     Purchase ps
     on pn.id = ps.person_id
      join
     Item i
     on i.id = ps.item_id
    )
    where price in(22000, 55000)
    ;

This sql will add to the result of the previous statement columns from the `Item` table and filter by price the resulting set.

Here is the result set with the table it came from at the top:

    Person         | Purchase                      | Item
    id name city   | person_id item_id item_count  | id price
    1  John lucerne  1         2       300          2  55000
    2  Adam wilton   2         3       200          3  22000
    1  John lucerne  1         2       70           2  22000
     \--------------/
         1st join               \------------------/
                                      2nd join

This explanation is about inner join, but there are also left and right outer joins.

**Left outer join** brings all the rows from the left table even if the on condition doesn't apply.

Likewise, **right outer join** brings all of the rows from the right table, even if the on condition doesn't apply.

There is also a **Cartesian join**, this is basically a Cartesian product of both tables, for each row `a` in the first table, there is a pair with every row from the second table in the result set.
