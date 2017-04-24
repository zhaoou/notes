# Spring notes: Java config

One last option for bean configuration in spring framework, is java based configuration.

Java config allows us to use java to define beans and dependencies of the application.

To enable **java config** we need to create a class annotated with @Configuration annotation. Inside of this class we define methods that create and return **beans to be registered in the application context**.

    @Bean
    public Apple redDelicious(){
        return new RedDelicious();
    }

Code above registers a bean with id = redDelicious in the context. The name is derived from the method name, this can be overriden by providing the name explicitly:

    @Bean(name="red")
    public Apple redDelicious(){
        return new RedDelicious();
    }

To enable **dependency injection using java config** we have to recall that all beans by default are singletons.

    @Bean
    public Pie applePie(){
        return new Pie(redDelicious());
    }



Spring intercepts all calls to redDelicious() and returns the same instance.

An alternative way to define a dependency is to simply declare the dependency as a parameter to the @Bean annotated method:

    @Bean
    public Pie appliePie(RedDelicious red){
        return new Pie(red);
    }


*Spring will autowire (by type) a bean of type RedDelicious into this method.*

That is all there is to know about java config.
