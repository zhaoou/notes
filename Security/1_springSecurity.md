# Spring security foundations

Two basic mechanisms spring security uses to secure your app are:

* servlet filter
* method security

#### Servlet filter

```
               |------------------------|     |------------------|
httpRequest--->| spring security filter |---->| dispatch servlet |
               |----------------\-------|     |------------------|
                                 \
                              (delegates to)
                                    \
                                     \
                        | spring security filter chain |

```


#### Method security
using a proxy object, spring wraps target objects and applies security aspect.


```
              |--------------proxy----------------------------------|
              |                                                     |
              |    *******************         |----bean---------|  |
method call-->|----***   spring    ***----\    |                 |  |
              |    ***  security   ***     \---|---->*method*    |  |
              |    *******************         |                 |  |
              |                                |-----------------|  |
              |                                                     |
              |-----------------------------------------------------|
```


Configuration of both **method invokation security** and **web security** requires us to help spring understand our users and what they can and cannot do.

To do that, spring needs:

* UserDetailsService, its' only method returns UserDetails from username.
* UserDetails returns a list of authorities this user has + other details.

**Authority: a role OR a fine grained definition of what a user can/cannot do.**

What commonly occurs in practice, is that User entity implements UserDetails, and stores user roles (as another entity: Role, or as a String).
OTOH, UserService, which every app should have, implements UserDetailsService.

With this information we can configure web security and configure method security.
### Configure web security
We need to create a @Configuraiton class extending WebSecurityConfigurerAdapter and annotate it with @EnableWebSecurity.
```
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter{

@Autowired
UserDetailsService userService;

/* add passwordEncoder bean */
@Bean
public PasswordEncoder passwordEncoder() {
return new BCryptPasswordEncoder();
}

/* tell spring which userdetailsservice and passwordEncoder to use */
@Autowired
public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
auth.userDetailsService(userService).passwordEncoder(passwordEncoder());
}

/* let all requests to this path go through */
@Override
public void configure(WebSecurity web) throws Exception {
web.ignoring().antMatchers("/assets/**");
}

/* here we actually configure authentication and authorisation:
   only ADMIN can access /admin/** urls
   both ADMIN and USER can access any other url.
   login page is at /login and anybody can access it
   on success we redirect to /main
   on failure we redirect back to /login
   logout is at /logout
   on logout we redirect to / and delete cookie and invalidate session
   and on all forms we want csrf token.
*/
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

```

### Configure method security
Add another config file, in the most simple form it looks like this:
```
@Configuration
@EnableGlobalMethodSecurity(securedEnabled=true)
public class MethodSecurityConfig
extends GlobalMethodSecurityConfiguration {}
```

And here is how you add security to a methods:
```
@Secured({"ROLE_ADMIN"})
public void addUser(User user) {
```

### View security
Two common things we want to do in views are:

* Show information about current user   
* Conditional rendering of template parts

##### Show information about current user
```
<div sec:authentication="name">
  The value of the "name" property of the authentication object should appear here.
</div>
```
##### Conditional rendering of template parts

Basic:
```
<div sec:authorize="hasRole('ROLE_ADMIN')">
  This will only be displayed if authenticated user has role ROLE_ADMIN.
</div>
```
Is this user authorised to visit this URL?
```
<div sec:authorize-url="/admin">
  This will only be displayed if authenticated user can call the "/admin" URL.
</div>
``` 
Is this user authorised to POST to this URL?
```
<div sec:authorize-url="POST /admin">
  This will only be displayed if authenticated user can call the "/admin" URL using the POST HTTP method.
</div>
```
Since thymeleaf security is not included in thymeleaf, we also need to add a dependency:
```
org.thymeleaf.extras:thymeleaf-extras-springsecurity3:2.1.1.RELEASE
```
And add a namespace to the views:
```
<html xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity3">
```
Cooooool.


