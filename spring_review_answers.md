# Spring review questions

## Core Spring

* What is spring

Spring is a dependency injection and aspect oriented programming framework that allows us to wire(connect) 
different parts of the application to create a complete application.

* Why use spring over regular Java

Spring allows us to define dependency declaratively, whereas without spring each component would need to find its dependencies.

* What is DI?

Dependency Injection, a mechanism that provides references to beans where we request them. Can be setter injection and constructor injection.
Use constructor to define mandatory dependencies and setter injection for optional dependencies.

* 3 ways to configure spring container, describe each in detail

(1) XML, (2) javaconfig, and (3) autowiring+component scan.

XML: define beans and wiring in 'beans' element of an xml document, create application context using this xml document.

Javaconfig: define and wire beans in a java @Configuration file, use this file to create application context.

Autowiring+scan: Define beans using @Component, define dependencies using @Autowired, add @Configuration class with @ComponentScan, use this class to create context.

* Describe default bean naming in all 3 configuration types, and how to override these defaults

In XML: bean name(aka id) is class name with a generated number, override by adding id attribute in bean declaration.

In Javaconfig: bean name is the name of the method annotated with @Bean, override by adding @Bean(name="john")

In Autowiring+scan: bean name is its class name (User -> user). override: @Component("john").

* name all possible ways to mix configurations in spring

**Use beans defined in XML and Javaconfig in autowiring:**

XML         -> autowiring: without additional configuration

Javaconfig  -> autowiring: without additional configuration


**Use beans defined in javaconfig, xml, autowiring+scan in javaconfig:**

Javaconfig  -> javaconfig: @Import(Config.class)

XML         -> javaconfig: @ImportResource("classpath:config.xml")

autowiring  -> javaconfig: add @ComponentScan on javaconfig file


**Use beans defined in xml, javaconfig, componentscan in xml:**

xml           -> xml: <import resource="other_config.xml" 

javaconfig    -> xml: declare a bean of config type <import resource="config.xml" />

componentscan -> xml: scan for components <context:component-scan base-package="a.b" />


* name 4 bean scopes, which ones are pertinent to SpringMVC?

Singleton - all beans are singletons by default, spring wires reference to the same bean,

Prototype - new instance for each dependency is created,

Session - new instance per web session,

Request - new instance per web request

* what is AOP?

Aspect oriented programming: a way to add functionality without changing the source code where we add that functionality.

* How do we use AOP in spring, give 3 examples:

Transactions, Security, Logging.

* Describe domain language used in describing aspects:

Joinpoints: all possible places where we can add functionality

Pointcut: subset of joinpoints where we chose to add functionality

Advice: action that should happen before-after-around-afterthrowing on a pointcut

Aspect = advice + pointcuts.

* How to start spring context in a main method? 

Javaconfig:

	ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);			
	Game game = context.getBean("cricketGame", Game.class);
	System.out.println(game.playGame());
		
XML:		

	ApplicationContext context = new ClassPathXmlApplicationContext("context_config.xml");
	Game game = context.getBean("game", Game.class);
	System.out.println(game.playGame());

## JDBC

* Describe JDBC without spring

DriverManager > Connection > Statement > ResultSet

Connection c = DriverManager.getConnection("jdbc:mysql://localhost/test?user=admin&password=pass");

from connection we get Statement

executing query on the statement gives us result set

we can iterate over result set( of rows) to get values of column in that row.

* Describe how to perform joins in SQL(name some common joins)

Joins merge 2 or more tables based on criteria:
```
select * from (User u join Role r on u.id = r.user_id);
```

* Describe transactions in SQL/JDBC

Transactions are all or nothing operations on the database.
We group actions that should all be performed as unit of work in a transaction.

In SQL: we use begin and commit keywords to manage TX.

In JDBC: connection.setAutocommit(false) and connection.commit() or connection.rollback() on SQLException

* Describe 3 concurrency phenomenas that can be observed in the database

Dirty read - TX_A sees data written by TX_B as it is being written and before it is committed.

Fantom read - TX_A uses aggregate function (like sum()) twice and gets different results because TX_B added rows between reads.

Non-repeatable read - TX_A reads a value of one row twice and gets different result because TX_B changed that value between reads.

* Describe transaction isolation levels

read uncommitted: dirty read, non-repeatable read, fantom read

read committed: <s>dirty read</s>, non-repeatable read, fantom read

repeatable read:  <s>dirty read, non-repeatable read</s>, fantom read

serializable  <s>dirty read, non-repeatable read, fantom read</s>

* Describe how we can model many-to-one relationship in the DB

One table (A) has a foreign key reference to another table(B). Table A can have 0, 1 or many rows pointing to the same entry in table B.

* Describe how to model many-to-many relationship in the DB

Join table contains foreign keys to two tables with many-to-many relationships.

* Describe Spring's approach to JDBC, name key interfaces/classes

Spring removes code that is not essential, and leaves us only things we actually have to do. This is seen in JdbcTemplate.

Key interfaces: 

DataSource - like DriverManager but with connection pool.

JdbcTemplate - used for performing CRUD operations and obtaining results.

RowMapper - SAM interface for extracting data from each row of result set. applicable to reads only.

* What is a repository?

Repository is a design pattern that separates code using the objects from code responsible for persistence of these objects.
Repository implementation knows how exactly the objects are stored. We can have one Repository interface and several implmentations each using different persistence mechanism.

* What is a datasource? How is it different from drivermanager?

DataSource represents one datasource in our program. Often a database. DataSource supports idea of **connection pool**, where connections are never closed but kept open to improve speed of connection to the database.

* Describe steps to configure JDBC based repository in Spring

1) configure Datasource: jdbc url, username and password, connection pool configuration
2) create a Repository with a JdbcTemplate field by wiring datasource into JdbcTemplate.

## JPA

* What is JPA?

Java ORM framework specification.

* What is Hibernate? compare Hibernate and JPA

Hibernate is an implementation of JPA. Hibernate can be used directly without JPA, while JPA requires an implementation, like Hibernate.

* Name key classes/interfaces in JPA

EntityManagerFactory creates EntityManager. EntityManager is used to create queries and perform CRUD operations.

* What are entities?

Entities are nouns in the domain model. They represent an idea from real world that we represent as an object of a class.

* Name entity states

new-managed-removed-detached

* What are value types? 

Basic types like String and classes annotated with @Embeddable. They don't require and identity, their identity is their value.

* How can an entity store its collection of values types, what about one value type?

An entity can store a collection of value types by using @ElementCollection annotation on that collection. Using @Embedded we can store one value type.

* Name 2 main types of relationships between entities?

Unidirectional and bidirectional

* Name 4 main cardinalities.

@OneToOne, @OneToMany, @ManyToOne, @ManyToMany

* Name 7 possible combinations of cardinalities and directions. Why there are only 7 and not 8?

Unidirectional @OneToOne, Unidirectional @OneToMany, Unidirectional @ManyToOne, Unidirectional @ManyToMany.

Bidirectional @OneToOne, Bidirectional (@ManyToOne-@OneToMany), Bidirectional @ManyToMany.

Bidirectional @ManyToOne and @OneToMany become two sides of the same relationship.

* What is unique about unidirectional relations, who owns the relation?

In unidirectional relationships, the entity with the @\*nyTo\*ny annotation owns the relationship.

* What is unique about bidirectional relations? who owns the relations?

In bidirectional relationships, in @ManyToOne relationships, many side always owns the relationship. In @ManyToMany and @OneToOne, we have to decide the owner by designating the inverse side. Inverse side of the relationship should have 'mappedBy' attribute pointing to the owning side.

* How do we mark an owning side in a bidirectional relation? What is inverse side?

Inverse side is the other, non-owning, side of the relationship. Annotation on the inverse side has a 'mappedBy' attribute.

* What is eager/lazy loading?

Lazy loading means that referenced objects are fetched as they are needed, if the session is open.
Eager loading means that referenced objects are fetched together with the owner. Egerly loaded objects can be used when the session is closed.

* What is JPQL?

SQL like, object oriented query language.

* name 2 main types of queries

Dynamic and static.

Dynamic: em.createQuery("SELECT c FROM Customer c WHERE c.name LIKE :custName"); 

Static queries use @NamedQuery defined on entity, and em.createNamedQuery("namedQueryName");

* What is @NamedQuery, where do we place it?

@NamedQuery defines a static query. We usually place it on the entity it belongs to.
```
@NamedQuery(
 name="findCustomersByName",
 query="SELECT c FROM Customer c WHERE c.name LIKE :name"
)
```

* What is native query?

A query using SQL. `em.createNativeQuery(“select * from item”, Item.class);`

* name 2 types of parameters in JPQL, what simbols each one uses?

Named and positional. `:name` is a named parameter and `?1` is a positional one.

* What is pagination?

Pagination allows us to controll what subset of the result is returned:

```
Query query = em.createQuery("select i from Item i");
query.setFirstResult(40).setMaxResults(10);
```

* What is JOIN FETCH?

JOIN FETCH allows us to fetch relationships eagerly as the query is run.

* What nuances we need to know about JOIN FETCH? (you run a query without success, despite entities being present in the DB?)

By default, JOIN FETCH will work as an inner join, that is it will only return entities that have the relationship. If some entity doesn't have a relationship (has zero relationship), it will not be fetched.

```
SELECT e FROM Employee e JOIN FETCH e.address
```
The query above will not return employees without address.
To force the fetch of all of the employees, even the ones without the address, we need to use LEFT JOIN FETCH:

```
SELECT e FROM Employee e LEFT JOIN FETCH e.address
```

* What is spring data JPA?

Spring data JPA is a project of Spring framework, that automatically implements JPA repositories based on convention. We have to provide an interface and Spring data JPA will implement a generated repository and wire it anywhere it is needed.

* How to configure JPA in Spring without Spring Data JPA: describe all needed beans and steps.

1) configure DataSource

2) configure JpaVendorAdapter

3) configure EntityManager and give it DataSource and JpaVendorAdapter

4) configure JpaTransactionManager by giving it EntityManager

5) enable JPA annotations in repositories by adding PersistenceAnnotationBeanPostProcessor bean to container.

## Transactions

* What is a transaction?

Set of actions that either all succeed or all fail.

* How does spring handle transactions?

Spring uses AOP to wrap all beans with tx methods. AOP begins a tx, and decides to commit or rollback based on presense of RuntimeException.

* What 2 main types of transaction do we have?

We have local and distributed txs. Local tx uses one resource, distributed uses 2 or more resources.

* What is JTA transaction manager? When do we need it? Name 2 JTA transaction managers.

JTA tx manager is tx manager capable of managing distributed transactions. Bitronix and atomicos are 2 examples.

* 2 main ways to manage transactions?

Declarative and programmatic: declarative - we tell container where tx begins and ends, programmatic - we write code that performs tx ourselves.

* Describe Java exception hierarchy, what are checked/unchecked exceptions?

Checked exception is the one where JVM is checking for exception handling code (catch block).
Unchecked exception is the one where JVM does not check for exception handling code.

Checked can be considered recoverable, that is JVM can fix it by running code in catch block, whereas unchecked is unrecoverable.

Checked exceptions require try-catch block, whereas unchecked don't. We still can catch unchecked exceptions if wish to do so.

```
Throwable interface
    |          	 |
  Error   	 Exception
                 |       |
    RuntimeException    ...


```
Error and RuntimeExceptions are unchecked, we don't have to provide try-catch block.

Any Exception, Exception subclass, that is not RuntimeException are checked, require try-catch block.


* In spring declarative transactions, when is transaction marked for rollback?

If during the execution of tx, a RuntimeException was thrown.

* How to specify Tx isolation level in Spring? Why do we specify it? Give an example.

`@Transactional(isolation=Isolation.SERIALIZABLE)` placed on tx method or a class will specify serializable isolation level.
We specify it to define the consistency of data that this transaction can see, with regards to other transactions running simultaneously.

If we are calculating a report that runs a summary function twice, we can be succeptible to phantom reads. To make sure we don't have phantom reads we set isolation to serializable, as this is the only isolation level guaranteeing that.

* How to specify Tx propagation level in spring, why do we specify it? give an example.

`@Transactional(propagation=Propagation.REQUIRES_NEW)` placed on a method or a class will specify this propagation level.
We can specify it to define what happens when some other method is calling this method.

In this case:

If that calling method already was in transaction, that tx will be suspended and new one will be created around this method.

If the calling method was not in tx, new one will be created around this method.

**Example**
Lets say we have a service method `performPurchase()` that removes an item from inventory, and records a sale.
SaleRepository and InventoryRepository have default propagation level (REQUIRED).

If `performPurchase` is not marked as @Transactional, then we don't have all-or-nothing, unit of work behavior.
We need to mark `performPurchase` as @Transactional.

If we want to keep track of all attempted purchases, even if they fail, we need to define another method: `recordSaleAttempt`, in some repository and mark it as REQUIRES_NEW. This will force it to be independent of the `performPurchase` transaction, and even if `performPurchase` method is rolled back, `recordSaleAttempt` will commit.


* Explain, and give examples of, REQUIRES_NEW, REQUIRED, and SUPPORTS propagation levels.

REQUIRES_NEW:

if calling method already is in tx, we suspend it and start a new tx, around current method.
if calling method is not in tx, we create and start a new tx, around current method.

REQUIRED:

if calling method is in tx, we join that transaction
if calling method is not it tx, we create new tx. 
This is default behavior, and all repository method behave this way.

SUPPORTS:

joins a tx if it is present in the calling method,
runs without tx if it is not present.


* What is readOnly property of a Tx, what about timeout property?

* What is default Isolation and propagation level in spring transaction?

* Present a demo project showcasing Spring transaction details.

* How to configure beans needed for TX support in spring?

* What role does repository play in spring transactions?

* What role does service play in spring transactions? Give an example.


## MVC

* What is servlet?

* What 2 main abstractions are used with servlet?

* What is servlet container? One example of.

* What is web.xml?

* What is JSP?

* What is dispatch servlet?

* Who creates dispatch servlet?

* What is AbstractAnnotationConfigDispatcherServletInitializer?

* What 3 main actions does AbstractAnnotationConfigDispatcherServletInitializer perform?

* How does container find AbstractAnnotationConfigDispatcherServletInitializer?

* What 2 main contexts are need for SpringMVC?

* What is the relationship between 2 main contexts in SpringMVC?

* What functionality logically belongs to the root(core) context? Name a few bean types that live there.

* What functionality logically belongs to the web context? Name a few bean types that live there.

* How to configure web context? 

	@Configuration
	@EnableWebMvc
	@ComponentScan("web")          // web related beans
	public class WebConfig extends WebMvcConfigurerAdapter {}
	

* What is view resolver? 

* What is servlet filter? How is it similar to AOP?

* Describe request flow through SpringMVC framework, in as much details as possible, draw a diagram.

* What is a controller?

* How to configure a controller? What annotation can we have in a controller?

* What is ui.model?

* What is a view template?

* What is a templating engine.

* How is ui.model used with templating engines?

* What 2 main http methods do you know? 

* What is the difference between 2 main http method?

## REST

* What is REST?

* What do we mean by 'state'

* Who are the main users of the REST?

* what is http request header?

* What is content negotiation?

* What is message conversion?

* What role does view resolver play in message conversion in REST?

* What 2 main roles exist in REST web service?

* What does it mean to be an endpoint?

* What does it mean to be an endpoint consumer?

* What is a REST template?

* How to use REST template to GET resource?

* How to use REST template to POST resource? What does it mean to post a resource?


## Security

* What 2 main approaches does spring security use to secure our applications?

* What is an authority?

* What is a cookie?

* What is a session?

* What 2 main interfaces help spring security understand a user based on the username? 

* How can we implement these 2 main interfaces in our application?

* What is a password encoder?

* Describe main url routes used by spring security

* What is a success/failure handler?

* How to utilize spring security in views? 

* Explain each line of the following code:

	@Configuration
	@EnableWebSecurity
	public class SecurityConfig extends WebSecurityConfigurerAdapter{

		@Autowired
		UserDetailsService userService;

		@Bean
		public PasswordEncoder passwordEncoder() {
			return new BCryptPasswordEncoder();
		}

		@Autowired
		public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
			auth.userDetailsService(userService).passwordEncoder(passwordEncoder());
		}

		@Override
		public void configure(WebSecurity web) throws Exception {
			web.ignoring().antMatchers("/assets/**");
		}


		@Override
		protected void configure(HttpSecurity http) throws Exception {
			http
				.authorizeRequests()
				.antMatchers("/admin/**").hasRole("ADMIN") // order is important
				.antMatchers("/**").hasAnyRole("ADMIN", "USER")
			.and()
				.formLogin()
				.loginPage("/login")
				.permitAll()
			.successHandler(
		(request, response, authentication) -> response.sendRedirect("/main"))
			.failureHandler(
		(request, response, authentication) -> response.sendRedirect("/login"))
			.and()
				.logout().logoutRequestMatcher(new AntPathRequestMatcher("/logout"))
				.logoutSuccessUrl("/").deleteCookies("JSESSIONID")
				.invalidateHttpSession(true)
			.and()
				.csrf();
		}
	}

* What is method security?

* What does this do:

	@Secured({"ROLE_ADMIN"})
	
* Explain each line of this code:

	@Configuration
	@EnableGlobalMethodSecurity(securedEnabled=true)
	public class MethodSecurityConfig extends GlobalMethodSecurityConfiguration {}
	
* Now read this in Kevin Malone's voice: "Nice"
