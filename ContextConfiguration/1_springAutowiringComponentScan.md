# Spring notes: Autowiring and Component scan.


In the next few month, I will be posting my Spring framework notes here, for all to see and for me to review. Mostly for me to review, but if anybody finds them helpful, you are welcome:)

Spring framework is the sanest way to build software that I know, and I only learned to appreciate this fact by working in many companies that keep reinventing the wheel, usually in the form of a unmaintainable spaghetti code that eventually is so hard to change and maintain that company's success and well being becomes endangered by its' own product.

Any nontrivial piece of software consists of multiple classes, that work together to solve business problems, or to implement use cases.

One approach to connecting these objects, is dependency injection. This means that objects know about each other only through interfaces, and that there is a separate entity responsible for providing concrete implementations where needed.

Spring is one such framework. Modern Spring uses component scanning and autowiring to satisfy dependencies any given objects has, and keeps code clean.

*Component scanning* is a process in which spring scans the project, and finds all classes that we mark as components (or parts) of our application.

*Autowiring* is a process in which spring finds all the places in which we need an instance of a certain class, and satisfies this dependency by injecting an instance of that class.

All of this is achieved, amazingly, by only a handful of annotations: @Component, @Autowired, @ComponentScan and @Configuration.

**@Component**

@Component indicates to spring that this class is a component of the application. By default, all objects are singletons, and only one instance of a class is created. In spring context, every object is given an id, which defaults to the class name with first letter lowercased.

We can override the default name:

    @Component("dodo")
    public class A{/* class body */}


**@Autowired**

@Autowired indicates to spring that there is a dependency it has to satisfy. Since all beans default to being singleton, all places in the code that require a particular object will be injected with a reference to the same object.

@Autowired can be placed on a *constructor* of a class that needs that dependency:

    @Autowired
    public A(B b){/* constructor body */}


@Autowired can also be placed on any *method* that takes a bean as a parameter:

    @Autowired
    public void setB(B b){/* setter body */}

Optional dependency can be indicated by (required = false):

    @Autowired(required = false)

If a dependency is not optional, and cannot be satisfied by any of the beans spring found, an exception will be thrown.

Personally, I had a lot of luck by simply annotating a field with @Autowired. Spring was able to do its' magic and inject the field.


**@Configuration and @ComponentScan**

Two remaining pieces of the puzzle are @Configuration and @ComponentScan.

**@Configuration** marks a class that we use to configure spring, in the simplest case, it is an empty class.

**@ComponentScan** tells spring to perform component scan. A fitting place for this annotation is the class we tagged with @Configuration.

@ComponentScan scans current package, and all subpackages of that package by default, so if you place it high in the package hierarchy, no additional work is needed.

However, we can also provide one or more packages that spring must scan:

    @ComponentScan("com.a") //scan com.a package
    @ComponentScan(basePackages={"com.a", "com.b"})// scan 2 packages

Alternatively, we can tell spring to scan packages containing certain classes:

    @ComponentScan(basePackageClasses={A.class, B.class})

That's all there is to know about these annotations!
