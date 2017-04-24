# Transactions in Spring boot

It's official, I am in love with spring boot.
Smart people at Pivotal created an amazing framework! Initially, I was concerned that it is just an attempt to sell spring, but it is much much more than that.

I have only briefly used spring before, but I think it is a great way to develop. Not only is it a great way to quickly build production ready applications, it also teaches best practices of the app development.

As a fan of DDD, I believe that great software is possible  by understanding the problem domain, and that includes (often omitted) transactional logic of your application.

Not all apps need transactional logic, and the ones that do, should keep this aspect of the app as simple as possible. Spring and Spring boot does exactly that, allows one to add support for transactions in your product with minimal changes in your codebase. Simple.

So, what does one need to enable transactions?
The recipe is simple: **transaction manager + annotations!**

Transaction manager makes sure that a resource is managed in a transactional way. Managers for the most common resources are available:

* JDBC
* JPA
* Hibernate
* JTA

There are 2 major ways to configure which methods of the app are to be transactional, and that is ***xml*** and ***annotations***. I am told that many don't like xml; so annotations it is.

With this class
```
@Entity
public class Customer {
    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private long id;
    private String firstName;
    private String lastName;
...

```
and a standard CRUD repository (that spring data graciously implemented for us) we can look into understanding transactions.

The class below represents a service:

```
@Component
public class CustomerService {

    @Autowired
    CustomerRepository repo;

    @Autowired
    Second second;

    @Transactional
    public void save(Customer c){
        repo.save(c);
        throw new RuntimeException();
    }
...
}
```

Of importance here is the fact that throwing RuntimeException causes a transaction to rollback, and customer is not persisted, as expected from the transactional method.

Due to implementation details (using proxies) if ```save(Customer c)``` calls ***another method on the same instance or on another object*** that throws RuntimeError, the transaction will be rolled back.

We can observe that in all of the cases above, methods called from within transactional method do indeed participate in the transaction. This works irrespective of whether methods called from within transactional method themselves have ***@Transactional*** or not. Seriously, try it!

On the other hand, if no exception is thrown, the transaction is committed.

A few things to keep in mind:

* @Transactional on a class makes all methods transactional
* @Transactional may not work properly on methods that are not public
* Self-invokation of @Transactional doesn't create proper transaction. If a non transactional method calls a transactional method on the same instance, no transaction is created.
* Defaults rule!
* Documentation provides a lot more information on tuning your transactions, use it!
