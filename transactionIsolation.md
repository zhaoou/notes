# Transaction isolation

In the context of database transactions, isolation describes the ability of a client to connect to a database and interact with the database as if there were no other clients using the same database.

Concurrency is the ability of a database to support many clients using the database at the same time. There are always trade offs between isolation and concurrency: more concurrency = less isolation, and vice versa.

There are 4 isolation levels supported by the modern databases, and these levels are defined by 3 behaviours:

* dirty read
* phantom read
* non-repeatable read

**Dirty read** occurs when one transaction(A) can see and use the data written by another transaction(B) as it is written. If transaction B rolls back, then A used the data that doesn't exist outside of the transaction B.

**Phantom read** occurs when one transaction(A) uses a number of rows in a calculation, and new rows are inserted by transaction B. This new rows affect calculation in A. Example: We count an average of some rows in A. New rows are added by B. A counts an average again but this time the value is different.

**Non-repeatable read** occurs when one transaction (A) reads a value once. Another transaction (B) updates that value and A reads it again, but now the value is different from the first read.

Four database isolation level progressively allow more of the behaviours above to occur. Here are the levels:

* dirty reads, non-repeatable reads, phantoms  <-- **read uncommitted**
* ~~dirty reads~~, non-repeatable reads, phantoms  <-- **read committed**
* ~~dirty reads~~, ~~non-repeatable reads~~, phantoms <-- **repeatable read**
* ~~dirty reads~~, ~~non-repeatable reads~~, ~~phantoms~~ <-- **serializable**

We can control the isolation level of a transactions that we run. Once the transaction that we run commits or fails, the database goes back to the default isolation level.

When we use the database, we can decide what isolation should apply to all transactions (globally), and what transactions need their isolation level specified (locally). If we have a busy database and we are generating a report that calculates averages, then we might want to wait for all other transactions to insert rows into a table of interest by setting isolation level to serializable.

The more reliable we want data in a transaction, the higher isolation level we need. The higher isolation level we need, the lower the concurrency we get.

Combination of transactions and isolation levels allow us to fine-tune the database to our specific needs, but requires significant investigation and understanding of use case and application needs. Tradeoffs everywhere.

[good read on this topic](https://www.postgresql.org/docs/9.1/static/transaction-iso.html)
