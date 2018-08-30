## Bean Initialization


### 1) Load bean definitions 
>from XML, javaconfig or componentscan
* Bean definitions loaded into a bean factory and indexed by ids.
* BeanFactoryPostProcessor called and can modify bean definitions before object are created. Example: property placeholder `${url}`

### 2) Initialize bean instances
* Eagerly instantiate beans
* Dependency injection
* Post processing steps:
>* postProcessBeforeInitialization (from BeanPostProcessor interface)
>* initializer: @PostConstruct or initMethod
>* postProcessAfterInitialization (from BeanPostProcessor interface)

### 3) Ready to use

## Bean Usage

* Some beans may be wrapped in proxies by BeanPostProcessors
> Proxies can be JDK(Interface) and CGLIB(Subclass) based

## Bean Destruction

* @Predestroy or destroy-methods called on non-prototype beans when context is closed
> Register shutdown hook with JVM: `context.registerShutdownHook();`



