# Spring JPA queries

#### Basic Queries

 Query methods are in the EntityManager.
The find method can be used to find instances.

```
em.find(User.class, id);
```

#### JPQL

* is a language similar to SQL
* is used to create, search, update or delete entities
* has object-like syntax

The query below

* declares var `c`, of type `Customer` (an entity).
* searches for all Customers with this `custName`


```
SELECT c FROM Customer c WHERE c.name LIKE :custName
```

`:custName` must be assigned before the query is run!

#### Dynamic Queries

The `createQuery` method is used to create dynamic queries.
They are defined directly in the method.

```
 public List findWithName(String name) {
  Query q = em.createQuery(
  "SELECT c FROM Customer c WHERE c.name LIKE :custName");
  query.setParameter("custName", name);
  return query.getResultList();
}
```

#### Static Query (@NamedQuery)

The `createNamedQuery` method is used to create static queries.
They are declared using the @NamedQuery annotation.

Declared:
```
@NamedQuery( 
 name="findCustomersByName", 
 query="SELECT c FROM Customer c WHERE c.name LIKE :name"
)
```
Used:
```
public List findWithName(String name) { 
 Query query=em.createNamedQuery("findCustomersByName");
 query.setParameter("name", name); 
 return query.getResultList();
}
```

#### Native Query (SQL)

```
em.createNativeQuery(“select * from item”, Item.class);
```

#### Named Parameters in Queries

* Named parameters are prefixed with a colon `:`
* `setParameter(String name, Object val)` to set value

```
public List findWithName(String name) {
  return em.createQuery(
  "SELECT c FROM Customer c WHERE c.name LIKE :name")
  .setParameter("name", name)
  .getResultList();
}
```

Named parameters are case-sensitive and may be used by both dynamic and static (named) queries.

#### Positional Parameters in Queries

* Positional parameters are a question mark `?` followed the position number, from 1 and up.
* `setParameter(integer postn, Object val)` to set value

```
public List findWithName(String name) {
    return em.createQuery(
    “SELECT c FROM Customer c WHERE c.name LIKE ?1”)
    .setParameter(1, name)
    .getResultList();
}
```

Input parameters are case-sensitive, and may be used by both dynamic and static queries.

### Bonus topics

#### Pagination

Allows us to control what subset of the results is returned.

```
Query query = em.createQuery("select i from Item i");
query.setFirstResult(40).setMaxResults(10);
```

This can be used to process items in batches, can be useful for web pages and REST endpoints.


#### Join Fetch

The FETCH option can be used on a JOIN to fetch the related objects in a single query. This avoids additional queries for each of the object's relationships, and ensures that the relationships have been fetched if they were LAZY.

```
SELECT e FROM Employee e JOIN FETCH e.address
```
This statement has a little problem: it is an inner join. It **will not fetch employees that don't have an address** assigned.

We have to differentiate between inner joins and left outer join in this situation.
To fetch all employees, with and without address, use **left join fetch**:

```
SELECT e FROM Employee e LEFT JOIN FETCH e.address
```

Hint:
Pagination and Join Fetch are common solutions to n+1 problem.

#### Getting results

Here are the two most useful methods for getting results of the query:

```
query.getSingleResult();
query.getResultList();
```

There is also [Criteria API](http://docs.oracle.com/javaee/6/api/javax/persistence/criteria/CriteriaQuery.html), where you can describe what objects you need fetched.

And here is some great resources(which were used in writing this post):
http://docs.oracle.com/javaee/6/tutorial/doc/bnbtg.html
https://en.wikibooks.org/wiki/Java_Persistence/JPQL

###### Cheers!
