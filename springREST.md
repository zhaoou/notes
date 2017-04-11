# Spring REST

If you are familiar with REST, you might be wondering how to enable it in Spring.
All we have to do is configure ViewResolver to return JSON for our objects instead of HTML.

There are 2 main ways we can configure Spring to do that:

### Content Negotiation & Message Conversion

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
