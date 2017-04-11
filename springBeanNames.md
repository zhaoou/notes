# Default spring bean names


**All beans in a spring context have names (aka id).**

In each of the ( javaconfig | autowiring | XML ) there is a default name given to a bean, and a way to override that name.

###Javaconfig

Default
```
@Bean
pubic Team sox(){}

// bean type: Team
// bean name: sox
```
Override
```
@Bean(name="redSox")
pubic Team sox(){}

// type: Team
// name: redSox
```



###Autowiring

Default
```
@Component
class BBGame implements Game{}

// type: Game
// name: bBGame       <-- notice lowercase 'b'
```
Override
```
@Component("duck")
class BBGame implements Game{}

// type: Game
// name: duck
```
###XML

Default

```
<bean class="beans.Sox"/>

// type: Sox
// name: beans.Sox#1   <-- default naming scheme
```
Override:
```
<bean id="cubs" class="beans.Sox"/>

// type: Sox
// name: cubs
```

When mixing configurations, knowing these naming schemes allows us to refer to the correct beans.
