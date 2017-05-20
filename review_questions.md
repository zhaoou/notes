# Spring review questions

## Core Spring

What is spring

Why use spring over regular Java

What is DI?

3 ways to configure spring container, describe each in detail

describe default bean naming in all 3 configuration types, and how to override these defaults

name all possible ways to mix configurations in spring

name 4 bean scopes, which ones are pertinent to SpringMVC?

what is AOP?

How do we use AOP in spring, give 3 examples
Describe domain language used in describing aspects.
How to start spring context in a main method? 

## JDBC

Describe JDBC without spring
Describe how to perform joins in SQL(name some common joins)
Describe transactions in SQL/JDBC
Describe 3 concurrency phenomenas that can be observed in the database
Describe transaction isolation levels
Describe how we can model many-to-one relationship in the DB.
Describe how to model many-to-many relationship in the DB.
Describe Spring's approach to JDBC, name key interfaces/classes.
What is a repository?
What is a datasource? How is it different from drivermanager?
Describe steps to configure JDBC based repository in Spring

## JPA

What is JPA?
What is Hibernate? compare Hibernate and JPA.
Name key classes/interfaces in JPA
What are entities?
Name entity states
What are value types? (Basic types like String and classes annotated with @Embeddable)
How can an entity store its collection of values types, what about one value type?
Name 2 main types of relationships between entities?
Name 4 main cardinalities.
Name 7 possible combinations of cardinalities and directions. Why there are only 7 and not 8?
What is unique about unidirectional relations, who owns the relation?
What is unique about bidirectional relations? who owns the relations?
How do we mark an owning side in a bidirectional relation? What is inverse side?
What is eager/lazy loading?
What is JPQL?
name 2 main types of queries
What is @NamedQuery, where do we place it?
What is native query?
name 2 types of parameters in JPQL, what simbols each one uses?
What is pagination?
What is JOIN FETCH?
What nuances we need to know about JOIN FETCH? (you run a query without success, despite entities being present in the DB?)
What is spring data JPA?
How to configure JPA in Spring without Spring Data JPA: describe all needed beans.

## Transactions

What is a transaction?
How does spring handle transactions?
What 2 main types of transaction do we have?
What is JTA transaction manager? When do we need it? Name 2 JTA transaction managers.
2 main ways to manage transactions?
Describe Java exception hierarchy, what are checkec/unchecked exceptions?
In spring declarative transactions, when is transaction marked for rollback?
How to specify Tx isolation level in Spring? Why do we specify it? Give an example.
How to specify Tx propagation level in spring, why do we specify it? give an example.
Explain, and give examples of, REQUIRES_NEW, REQUIRED, and SUPPORTS propagation levels.
What is readOnly property of a Tx, what about timeout property?
What is default Isolation and propagation level in spring transaction?
Present a demo project showcasing Spring transaction details.
How to configure beans needed for TX support in spring?
What role does repository play in spring transactions?
What role does service play in spring transactions? Give an example.


## MVC
...

## REST
...

## Security
...


