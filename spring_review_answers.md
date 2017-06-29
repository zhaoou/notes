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

-readOnly when set on a tx that actually only reads data from the database, can allow database to optimize concurrency.

-timeout limits how long a tx can run, if a tx exceeds the timeout limit, an exception is thrown.

* What is default Isolation and propagation level in spring transaction?

Default isolation is determined by the settings in the DB engine. Propagation defaults to REQUIRED.

* Present a demo project showcasing Spring transaction details.

**Example**
Lets say we have a service method `performPurchase()` that removes an item from inventory, and records a sale.
SaleRepository and InventoryRepository have default propagation level (REQUIRED).

If `performPurchase` is not marked as @Transactional, then we don't have all-or-nothing, unit of work behavior.
We need to mark `performPurchase` as @Transactional.

If we want to keep track of all attempted purchases, even if they fail, we need to define another method: `recordSaleAttempt`, in some repository and mark it as REQUIRES_NEW. This will force it to be independent of the `performPurchase` transaction, and even if `performPurchase` method is rolled back, `recordSaleAttempt` will commit.

* How to configure beans needed for TX support in spring?



* What role does repository play in spring transactions?

Repository methods are run in a transaction, that is tx is required. Default propagation (REQUIRED) allows for a TX to be created when needed, or joining the transaction started in @Service methods marked as @Transactional.

* What role does service play in spring transactions? Give an example.

@Service methods start transactions when needed. If a repository method is called from a non-transactional service method, it will create a transaction due to REQUIRED propagation.


## MVC

* What is servlet?

Is a small program running in a web container

* What 2 main abstractions are used with servlet?

HttpRequest and HttpResponse

* What is servlet container? One example of.

Servlet container is a program that listens to incoming requests and delegates them to the appropriate servlets. Tomcat.

* What is web.xml?

web.xml is a web deployment descriptor. It provides information about the web app to the tomcat, like mapping between url and servlet responsible for handling that url.

* What is JSP?

Java Server Pages. HTML like syntax for designing web templates. It gets compiled into servlet by the container.

* What is dispatcher servlet?

Dispatcher servlet is a servlet that knows how to route the request.

* Who creates dispatch servlet?

In Spring, Spring MVC framework creates dispatch servlet.

* What is AbstractAnnotationConfigDispatcherServletInitializer?

AbstractAnnotationConfigDispatcherServletInitializer is an abstract implementation of WebApplicationInitializer.
Web container uses it to start our web application.

* What 3 main actions does AbstractAnnotationConfigDispatcherServletInitializer perform?

starts web context, starts core context, configures the dispatch servlet.

* How does container find AbstractAnnotationConfigDispatcherServletInitializer?

Tomcat scans to find any implementation of ServletContainerInitializer, like AbstractAnnotationConfigDispatcherServletInitializer.

* What 2 main contexts are needed for SpringMVC?

root and web. Root contains entites, services, persistence and integration beans. Web contains controllers, views, view resolver and other web related beans.

* What is the relationship between 2 main contexts in SpringMVC?

Web context can access all beans from core context, core context cannot access any beans in the web context. Ideally, 

* What functionality logically belongs to the root(core) context? Name a few bean types that live there.

Application business logic belongs to the root context. Repositories, services, transaction managers and others live in root context.

* What functionality logically belongs to the web context? Name a few bean types that live there.

All web related functionality belongs to the web context. Controllers, view resolvers, etc live in web context.

* How to configure web context? 

	@Configuration
	@EnableWebMvc
	@ComponentScan("web")          // web related beans
	public class WebConfig extends WebMvcConfigurerAdapter {}
	

* What is view resolver?

View resolver is a bean that takes ui.Model and a view name, and using templating engine renders a view.

* What is servlet filter? How is it similar to AOP?

Filters have access to request and response objects and can be invoked before and/or after the servlet. They can be used similarly to AOP beans.

* Describe request flow through SpringMVC framework, in as much details as possible, draw a diagram.

Dispatch servlet recieves request and routes it to the appropriate method in the appropriate controller.

Controller performs validation and invokes service method(s).

Service method performs business logic, like calling remote service or using repositories and entities.

Controller places result of service call into ui.Model and returns appropriate view name.

View resolver uses view name and ui.Model to render appropriate view and returns that view.

* What is a controller?

Controller is a class containing methods responsible for handling requests.

* How to configure a controller? What annotation can we have in a controller?

Controller is just a class with @Controller. Just like any other bean, it should be picked up by the componenet scan to work.

* What is ui.model?

ui.Model is a hashtable like datastructure used for holding objects needed for the view to render.

* What is a view template?

view template is an incomplete view that needs data to become complete.

* What is a templating engine.

Templating engine is a mechanism that takes templates and ui.model and returns complete views.

* How is ui.model used with templating engines?

ui.Model stores data that templating engine needs to render the views.

* What 2 main http methods do you know? 

GET and POST

* What is the difference between 2 main http methods?

GET is used to read data, and POST is used for manipulating data, often to create a resource.

## REST

* What is REST?

Representative State Transfer. An architectural approach for transfering application state between applications.

* What do we mean by 'state'

State is set of values of all entities in the system

* Who are the main users of the REST?

Other applications.

* what is http request header?

A set of key value pairs attached to a http request.

* What is content negotiation?

Content negotiation is a process used by the server to determine what MIME type to send back to the client. Server can use 'Accept' request header, or file extension requested, to negotiate content.

* What is message conversion?

Is process where Spring converts objects (or collections of objects) to desired representation, using message converter.
Likewise, we can take an object representation and convert it into object or collection of objects.

* What role does view resolver play in message conversion in REST?

View resolver is not used in message conversion, controller methods do not return object(s) directly, and do not return view name.

* What 2 main roles exist in REST web service?

Client and server. Client consumes web service provided by the server.

* What does it mean to be an endpoint?

It means that you are a server responding to GET and POST requests and process them accordingly.

* What does it mean to be an endpoint consumer?

You have a RestTemplate getting and posting resources to an endpoint. No server is needed.

* What is a REST template?

RestTemplate is a convinience class that takes endpoint URL and performs REST operations.

* How to use REST template to GET resource?

```
Contact[] response = new RestTemplate().getForObject(getAllUrl, Contact[].class);
```

* How to use REST template to POST resource? What does it mean to post a resource?

```
new RestTemplate().postForObject(postUrl, c, Contact.class);
```
POSTing a resource means we want to create this resource on that endpoint.

## Security

* What 2 main approaches does spring security use to secure our applications?

Servlet filter(Web security) and AOP(method security)

* What is an authority?

A user role or fine-grain definitions of what user can/cannot do.

* What is a cookie?

A cookie is a key value pair that we can store in the browser.

* What is a session?

A session is an object that is associated with a particular user using cookie. Only session id cookie is stored in the browser, session lives in the application container. We can use session as a Map to store random key value pairs as needed.

* What 2 main interfaces help spring security understand a user based on the username?

UserDetailsService and UserDetails.

UserDetailsService returns UserDetails from username.

UserDetails returns authorities that user has.

* How can we implement these 2 main interfaces in our application?

A convinient approach is to have UserService implement UserDetailsService, and User entity implement UserDetails.

* What is a password encoder?

Password encoder is a bean that performs initial hashing of the password, and helps use authenticate a user later, based on provided password.

* Describe main url routes used by spring security

Default login page is at /login, while default logout page is at /logout.

* What is a success/failure handler?

Is a SAM interface that tells spring security what should happen on authentication succes/failure.

* How to utilize spring security in views?

We can conditionally render parts of the views based on authority of Principal(currently authenticated user). We can also render his or her granted authorities.

* Explain each line of the following code:
```
	@Configuration
	@EnableWebSecurity  // turns on web security
	public class SecurityConfig extends WebSecurityConfigurerAdapter{

		@Autowired // provides UserDetailsService implementation.
		UserDetailsService userService;

		@Bean // declares PasswordEncoder bean
		public PasswordEncoder passwordEncoder() {
			return new BCryptPasswordEncoder();
		}

		@Autowired  // wires UserDetailsService and PasswordEncoder into spring security
		public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
			auth.userDetailsService(userService).passwordEncoder(passwordEncoder());
		}

		@Override // allows unrestricted access to web assets.
		public void configure(WebSecurity web) throws Exception {
			web.ignoring().antMatchers("/assets/**");
		}


		@Override
		protected void configure(HttpSecurity http) throws Exception {
			http
				.authorizeRequests()
				.antMatchers("/admin/**").hasRole("ADMIN") // only admin can access this urls
				.antMatchers("/**").hasAnyRole("ADMIN", "USER") // only admins and user can access other urls
			.and()
				.formLogin()	// use html form login
				.loginPage("/login") // located here
				.permitAll()	// and allow anybody to access this form
			.successHandler(
		(request, response, authentication) -> response.sendRedirect("/main")) // on success redirect to /main
			.failureHandler(
		(request, response, authentication) -> response.sendRedirect("/login")) // else try again
			.and()
				.logout().logoutRequestMatcher(new AntPathRequestMatcher("/logout")) // use /logout to logout
				.logoutSuccessUrl("/").deleteCookies("JSESSIONID") // and delete sesssio id cookie
				.invalidateHttpSession(true) // destroy session
			.and()
				.csrf(); // enable default csrf tokens on all forms.
		}
	}

```
* What is method security?

Method security allows us to limit who can call bean methods in the core container.

* What does this do:

```
	@Secured({"ROLE_ADMIN"})
```
Secures any method. Specifically, will only allow admin users to call it.

* Explain each line of this code:
```
	@Configuration	// @Configuration is a @Component, and will be picked up by the component scan.
	@EnableGlobalMethodSecurity(securedEnabled=true) // turns AOP proxiing of beans with @Secured.
	public class MethodSecurityConfig extends GlobalMethodSecurityConfiguration {}
```	
* Now read this in Kevin Malone's voice: "Nice"
