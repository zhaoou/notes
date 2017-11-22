# Database transactions

Any modern database server allows multiple connections.
Many applications can and do use a database at the same time. What does it mean? If all they do is read the data, it is impossible to have any issues.

What can happen if two clients: A and B also modify data at the same time?

If A is reading a price of a house for example, while B is changing it, what exactly should A see?

Database will read the price of the house, store it in memory, update it to whatever B tells it to, and write it back into the field. If A is reading the price before database has written new value, A sees the old price.

Issues like this come up very often and there are many great books covering it in great detail.

What I want to talk about is transactions. Transaction is a set of actions that either all succeed or all fail. And this is where many people get confused (I know I did). There is nothing particularly hard about this, we often want things to happen as a group, or be undone: When I buy coffee, I pay and get my coffee. If I don't pay I don't get coffee. If I don't get coffee I don't pay.

As we often record information in the database, for example we have a table ```sale``` and table ```coffee```, we want to record the fact that a client was charged amount of money, and that we have sold, and have less of, coffee.

This is an example of a database transaction: we want both of this facts reflected in the database together, but not separately. What happens if the client doesn't pay? No coffee will be sold. What happens if the coffee is not sold, client will not pay. This actions are a unit, and don't make sense outside of the unit.

Databases support transactions, we often use them without even knowing, in fact all interactions with the database are transactions! Most databases have a trick called ```autocommit``` that allows people to use databases even without understanding what a transaction is. We can take control of the transactions using a very simple keyword: ```commit```.
A transaction begins when we run SQL command ```begin``` and ends on ```commit``` keyword. ```Commit``` forces database to commit the transaction: apply all of the changes in the statements between the beginning of the transaction and commit keyword, and make them visible to other users. Committing a transaction enables autocommit mode, and once again every SQL statement will be in it's own transaction.

If ```sql``` is a SQL statement, then our interaction with the database looks like this:
```
begin; sql1; sql2; commit;
sql3;
begin; sql4; commit;
sql5; ...
```

* sql1 and sql2 belong to one transaction
* sql3 and sql5 also belong to transactions, but they are not controlled by us, but by the autocommit mode of the database.
* sql4 belongs to its own transaction that we control using keywords ``` begin``` and ```commit```.

If something happens, and we decide to not commit the statements that we have performed from the beginning of the current transaction, we can ```rollback``` the transaction, cancel it. None of the statements in our transaction will be executed, no changes in the database will take place. It is important to understand that we can ```rollback``` only the transaction we are currently performing. Once a transaction is committed, we cannot roll it back, and will have to manually undo changes that we don't need.

As we can recall, many clients can connect to a single database, and all of them will be performing their own transactions.

To make many transactions possible, and to keep our data reliable we have to follow ACID properties of the transaction:

* A-atomicity
* C-consistency
* I-isolation
* D-durability

Atomicity simply restates the definition of a transaction: all or nothing, either all of the statements succeed, or none of them do.

Consistency means that the rules we have about data will remain unbroken after transaction commits. So a coffee shop, doesn't, for example, both give coffee and pay money **to** a customer.

Durability means that once a transaction is committed, all of the changes are permanent and visible to other users. Even if the server crashes, when we restart it the results of the transaction are there.

Isolation is a big topic that I will cover next.
