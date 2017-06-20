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

Use beans defined in XML and Javaconfig in autowiring:

XML         -> autowiring: without additional configuration

Javaconfig  -> autowiring: without additional configuration


Use beans defined in javaconfig, xml, autowiring+scan in javaconfig:

Javaconfig  -> javaconfig: @Import(Config.class)

XML         -> javaconfig: @ImportResource("classpath:config.xml")

autowiring  -> javaconfig: add @ComponentScan on javaconfig file


Use beans defined in xml, javaconfig, componentscan in xml:

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

* Describe how to perform joins in SQL(name some common joins)

* Describe transactions in SQL/JDBC

* Describe 3 concurrency phenomenas that can be observed in the database

* Describe transaction isolation levels

* Describe how we can model many-to-one relationship in the DB

* Describe how to model many-to-many relationship in the DB

* Describe Spring's approach to JDBC, name key interfaces/classes

* What is a repository?

* What is a datasource? How is it different from drivermanager?

* Describe steps to configure JDBC based repository in Spring

## JPA

* What is JPA?

* What is Hibernate? compare Hibernate and JPA

* Name key classes/interfaces in JPA

* What are entities?

* Name entity states

* What are value types? (Basic types like String and classes annotated with @Embeddable)

* How can an entity store its collection of values types, what about one value type?

* Name 2 main types of relationships between entities?

* Name 4 main cardinalities.

* Name 7 possible combinations of cardinalities and directions. Why there are only 7 and not 8?

* What is unique about unidirectional relations, who owns the relation?

* What is unique about bidirectional relations? who owns the relations?

* How do we mark an owning side in a bidirectional relation? What is inverse side?

* What is eager/lazy loading?

* What is JPQL?

* name 2 main types of queries

* What is @NamedQuery, where do we place it?

* What is native query?

* name 2 types of parameters in JPQL, what simbols each one uses?

* What is pagination?

* What is JOIN FETCH?

* What nuances we need to know about JOIN FETCH? (you run a query without success, despite entities being present in the DB?)

* What is spring data JPA?

* How to configure JPA in Spring without Spring Data JPA: describe all needed beans.

## Transactions

* What is a transaction?

* How does spring handle transactions?

* What 2 main types of transaction do we have?

* What is JTA transaction manager? When do we need it? Name 2 JTA transaction managers.

* 2 main ways to manage transactions?

* Describe Java exception hierarchy, what are checkec/unchecked exceptions?

* In spring declarative transactions, when is transaction marked for rollback?

* How to specify Tx isolation level in Spring? Why do we specify it? Give an example.

* How to specify Tx propagation level in spring, why do we specify it? give an example.

* Explain, and give examples of, REQUIRES_NEW, REQUIRED, and SUPPORTS propagation levels.

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
