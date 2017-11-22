# Spring MVC contexts


#### Spring MVC uses front controller design pattern with the dispatch servlet receiving all requests.

To **start a web application** spring performs the following:

* Scans classpath to find implementations of WebApplicationInitializer like AbstractAnnotationConfigDispatcherServletInitializer.

* Uses configuration from AbstractAnnotationConfigDispatcherServletInitializer to **start web context, app context and configure the dispatch servlet.**

**Dispatch servlet** needs:

* AppContext (root) - context containing all 'global' beans, potentially shared among several dispatch servlets. Loaded by ContextLoaderListener.

* WebApplicationContext (child) - context containing all web related beans, specific to this dispatch servlet.

* Mapping - URL prefix telling tomcat to route all requests with this prefix to this servlet.

There are a few **important rules about the contexts**(root and child, as defined above):

* A child context inherits (can access and override) beans from root.

* Sibling contexts cannot access each others beans.


Here is an example of AbstractAnnotationConfigDispatcherServletInitializer from Spring in Action(SiA):

```
import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

public class OurWebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {
@Override
protected String[] getServletMappings() {
 return new String[] { "/" };
}

@Override
protected Class<?>[] getRootConfigClasses() {
 return new Class<?>[] { RootConfig.class };
}

@Override
protected Class<?>[] getServletConfigClasses() {
 return new Class<?>[] { WebConfig.class };
}

@Override // add as many filters as needed for this servlet
protected Filter[] getServletFilters() {
 return new Filter[] { new MyFilter() };
}
}

```

Given this class, to enable Spring mvc, we need to provide WebConfig and RootConfig classes.

Here is a WebConfig from SiA:
```
@Configuration
@EnableWebMvc
@ComponentScan("web")          // web related beans
public class WebConfig extends WebMvcConfigurerAdapter {

@Bean                          // JSP view resolver
public ViewResolver viewResolver() {
 InternalResourceViewResolver resolver =
 new InternalResourceViewResolver();
 resolver.setPrefix("/WEB-INF/views/");
 resolver.setSuffix(".jsp");
 resolver.setExposeContextBeansAsAttributes(true);
 return resolver;
}

@Override                      // static content handling
public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
 configurer.enable();
}
}
```


And RootConfig:

```

@Configuration
@ComponentScan(basePackages={"core"})  // not web beans
public class RootConfig {}
```


When all said and done here is how contexts will look like

```

  | ApplicationContext (root) |
               |
               |
               |
               |
| WebApplicationContext (child) |

```
WebApplicationContext inherited all beans from the ApplicationContext, and may have overridden some of them

#### Filters and Listeners and more on servlets.

**Filters** are invoked before and after the servlet, and can be used to inspect/modify request/response objects. AKA AOP of the web.

**Listeners** can be added to the servlet container to react to container events, like session creation.

As we know, all WebApplicationInitializer implementations are invoked on container startup.
We can register a servlet, its filters and listeners in such an implementation:

```
public class MyServletInitializer implements WebApplicationInitializer {

@Override
public void onStartup(ServletContext servletContext)throws ServletException {

```
adding servlet:
```
 Dynamic myServlet = servletContext.addServlet("servlet", MyServlet.class);
 myServlet.addMapping("/custom/**");
```
adding filter:
```

 Dynamic filter = servletContext.addFilter("myFilter", MyFilter.class);
 filter.addMappingForUrlPatterns(null, false, "/custom/*");

```
adding listener:
```
 servletContext.addListener(MyEventListener.class);
}
}

```
While rarely needed, with this approach we can register multiple servlets and WebAppContexts.

BTW, if we do register many WebAppContexts, the context will look like a tree:
```
             | app context |                <- parent
               /        \
              /          \
| webappcontext1 |    | webappcontext2 |    <- child1 & child2
```
