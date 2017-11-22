# Spring Transactions Deep Dive


Transactions allow us to fine tune concurrent access to our data sources.

## VID (Very Important Definitions)

* Transactional resource - something that can participate in a transaction, something supporting transaction. (Message Queue, Database, etc.)

* PlatformTransactionManager - spring bean that represents one transactional resource, and manages transactions on that resource.
PlatformTransactionManager needs reference to the resource it is managing transactions for.



### Understanding transactions in Spring
**Transactions are broadly classified into 2 main categories:**

* Local - using one transactional resource, like 1 database
* Global - using 2 or more TX resources, like 2 databases

Global transactions require JTA transaction manager to coordinate transactions over several resources. (examples: Bitronix, Atomikos).

**2 main ways to manage transactions in Spring are:**

* programmatic - we write transaction handling code
* declarative - we tell spring how we want transactions done

From this point on, I:

* am talking about **local declarative transactions**
* will abbreviate transactions as TX

To provide declarative TXs, Spring uses AOP to wrap your repository beans in the transactional boilerplate:

```
           |  <- call intercepted by the proxy
           |
-----------|--------- proxy ----------------------
|          |                                     |
|          |                                     |
|       (tx.begin())                             |
|          |                                     |
|          |                                     |
|  --------|---- bean ----------                 |
|  |       |                   |                 |
|  |       |                   |                 |
|  |    save(User user)        |                 |
|  |       |       |           |                 |
|  |       |       |           |                 |
|  |       |       |           |                 |
|  |     (OK)   (Exception)----|-(tx.rollback())-|--->
|  |       |                   |                 |
|  |       |                   |                 |
|  |-------|-------------------|                 |
|          |                                     |
|          |                                     |
|      (tx.commit())                             |
|          |                                     |
|          |                                     |
|----------|-------------------------------------|
           |

```

There are a few things to be aware in this approach:

* if a non-TX method calls a TX method on itself, no TX is created.
* only TX method calls from one bean to another create transactions.
* we can specify what exceptions cause OR don't cause a rollback.
* **'In its default configuration, the Spring Framework's transaction infrastructure code only marks a transaction for rollback in the case of runtime, unchecked exceptions; that is, when the thrown exception is an instance or subclass of RuntimeException. (Errors will also - by default - result in a rollback). Checked exceptions that are thrown from a transactional method do not result in rollback in the default configuration.'**

Traditionally, people describe TX using ACID properties, here it is explained in the Spring context:

* A - atomicity: group actions into a Unit Of Work. Either all succeed or all fail.
* C - consistency: invariants are not violated before and after TX
* I - isolation: how this transaction sees actions of other transactions
* D - durability: this a problem that transactional resources take care of.


##### Atomicity
To ensure **atomicity** in our transactional code, we need to establish boundaries of the transaction and clearly understand what actions should be taking place as a unit. Specifically, Java methods define TX boundaries.

Another thing we sometimes have to consider to ensure atomicity, is how a transaction is propagated.
Propagation happens when one TX method calls another TX method.

There are a few thing that can happen:


Method being called **SUPPORTS** transactions:

* method is joining existing transaction(if calling method is already in a TX)
* method runs without transaction, if there is none in the calling method

Method being called **REQUIRES** transactions:

* method is joining existing transaction(if calling method is already in a TX)
* method creates a TX if there is none

Method being called **REQUIRES_NEW** transaction:

* method is suspending existing transaction(if calling method is already in a TX)
* method creates new transaction, executes inside of the transaction and returns
* calling method resumes its transaction

Method being called needs a **MANDATORY** transactions:

* method is joining existing transaction(if calling method is already in a TX)
* method throws an exception if there is none

There are also other ways to propagate a TX, see the spring docs for more info :)

##### Consistency
Our transactions should ensure the state of the system is valid before and after transaction.

##### Isolation
To take care of proper **isolation** means to understand what is acceptable for our transaction to see with respect to what is being written by other transactions.

We can specify isolation and propagation we need, using @Transactional.

Some other important things we can control using @Transactional:

* **readOnly** - if a transaction is read only, DB system can use the fact that READ locks don't conflict and optimize this query. Use this on all queries that don't change data.
* **timeout** - specify maximum time it is permissible for this transaction to run.

### Enabling transactions in Spring

1) Add **@EnableTransactionManagement** on @Configuration class. This turns on TX proxies.
2) Define **DataSource** ds and **JpaVendorAdapter** jpava.
3) Use ds and jpava to create an **EntityManagerFactoryBean** emfb.
4) Use emfb to create **JpaTransactionManager**
5) **@Transactional** on a repository class or methods
6) **PersistenceAnnotationBeanPostProcessor** to enable JPA annotations in repositories
7) Add **PersistenceExceptionTranslationPostProcessor** bean to enable exception enhancements

Have fun!
