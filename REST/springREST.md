# Spring REST


## First, lets talk about providing a REST service.

If you are familiar with REST, you might be wondering how to enable it in Spring.
All we have to do is configure ViewResolver to return JSON for our objects instead of HTML.

There are 2 main ways we can configure Spring to do that:

### Content Negotiation & Message Conversion
(I prefer a separate REST controller using messsage conversion)

#### Content Negotiation
Requires ContentNegotiatingViewResolver .

ContentNegotiatingViewResolver:

* determines requested content type from file extension first, and *Accept* header second.
* resolves the view by picking a first match from a list of candidate views.

We can influence ContentNegotiatingViewResolver by giving it a ContentNegotiationManager.

ContentNegotiationManager can:

* specify default content type
* ignore *Accept* header

ContentNegotiationManager is configured in WebMvcConfigurerAdapter:
```
@Override
public void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
 configurer.defaultContentType(MediaType.APPLICATION_JSON);
}
```
and given to ContentNegotiatingViewResolver :
```
@Bean
public ViewResolver cnViewResolver(ContentNegotiationManager cnm) {
 ContentNegotiatingViewResolver cnvr = new ContentNegotiatingViewResolver();
 cnvr.setContentNegotiationManager(cnm);
 return cnvr;
}
```
#### Message Conversion

* Spring can convert objects to and from some desired representations using Message Converters.
* There is a number of them wired by default; we can declare the additional ones.
* Converters rely on the corresponding library being present on the classpath.

To enable converters in SpringMVC we need to modify our controller methods:
```
@RequestMapping(method=RequestMethod.GET, produces="application/json")
public @ResponseBody List<Item> items(){ /* return items; */}
```

By adding `produces="application/json"` we indicate that this method can only produce that MIME type.
`@ResponseBody` tells spring that the returned value of this method should be returned to the client as a resource. The type of the resource is determined, again, by the *Accept* header.

This takes care of conversion **from** POJOs, but we also need to convert **to** POJOs.

```
@RequestMapping(method=RequestMethod.POST, consumes="application/json")
public @ResponseBody Spittle saveSpittle(@RequestBody Spittle spittle) {
  return spittleRepository.save(spittle);
}
```

Here, `consumes` and `@RequestBody` specifies that Spring needs to find a converter from JSON and convert the JSON into instance of Spittle.

To issue an appropriate POST request for this method, we should specify `Content-Type` and provide valid JSON in the body of the request.

Here is a Message Converter summary:

```
   Client                             Spring
__________________________________________________

GET
Accept='application/json'

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

                               1) produces="application/json"
                               2) @ResponseBody
                               3) MessageConverter converts POJOs to JSON

<<<<<<<<<<<<<<<<<<<<<<<<<<<<<

HTTP response with JSON

__________________________________________________

POST
Content-Type='application/json'
body with JSON

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

                              1) consumes="application/json"
                              2) @RequestBody on parameter
                              3) MessageConverter converts JSON to POJO

<<<<<<<<<<<<<<<<<<<<<<<<<<<<<

OK!

__________________________________________________
```

We can also  use @RestController on the controller. This is equivalent to having @ResponseBody on all of the methods in that controller.

Presence of @RestController doesn't remove the need for @RequestBody, where we need to convert JSON to object.

### Error handling in Spring REST

Common requirement is to handle errors. We need to:

1) Define Exceptions (extending RuntimeException)

2) Throw our exceptions from the controller

3) Define @ExceptionHandler methods in the controller, or in @ControllerAdvice annotated class.

For example:

```
@ExceptionHandler(UserNotFoundEx.class)
@ResponseStatus(HttpStatus.NOT_FOUND)
public Error handleUserNotFound(UserNotFoundEx ex){
  return new Error("user not found");
}
```


## Below we cover REST consumption

Using RestTemplate we can issue GET and POST requests to a REST endpoint. We usually use GET to get a resource and POST to create a resource on the endpoint.

Java API for JSON Binding is a spec, and an implementation of choice is Jackson. 

Jackson works both ways: 

* class + JSON -> object
* object + class -> JSON

So, if we want to get a Java object from JSON, we need to have a class defining that object. Often, we have to design one by looking at JSON.

This is another example of message conversion in Spring.

With this in mind, using RestTemplate is straighforward:

```
/* Fetch a contact resource by id from a given endpoint(defined by url) */

String getUrl = "http://localhost:8080/SpringRestDemo-0.0.1-SNAPSHOT/contact/{id}";
public Contact getContact(Long id){
	return new RestTemplate().getForObject(getUrl, Contact.class, id);
}

/* Create a contact resource at the endpoint */

String postUrl 	= "http://localhost:8080/SpringRestDemo-0.0.1-SNAPSHOT/contact";
public Contact postContact(Contact c){
	return new RestTemplate().postForObject(postUrl, c, Contact.class);
}

/* Get all Contacts from the endpoint */

private String getAllUrl = "http://localhost:8080/SpringRestDemo-0.0.1-SNAPSHOT/contacts/all";
public List<Contact> getAllContacts() {
	Contact[] response = new RestTemplate().getForObject(getAllUrl, Contact[].class);
	return Arrays.asList(response);
}
```

Yes, it's really that easy.
