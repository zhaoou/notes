# Spring: mixing configurations


With all of the varieties of configuration options available, spring allows us to go even further in that madness and mix configurations of different types.

*(Reminder, available types of configuration in spring are: (1) Autowiring and component scan, (2) xml, and (3) Javaconfig.)*

The solution that spring framework designers chose is very familiar to java programmers: **importing what's needed**.

Possible ways we can mix configurations are:

#### XML and/or java config beans in autowiring#####

* This happens automatically, no action is needed.

#### Javaconfig and/or xml defined beans in javaconfig####

* One javaconfig can use beans from another javaconfig by adding ``@Import({Config.class})`` to the javaconfig class where it is needed.

* Javaconfig can find beans defined in xml by adding ``@ImportResource("classpath:config.xml")`` to the javaconfig class.

* To add autowired beans to javaconfig, just add ``@ComponentScan`` to the javaconfig class.

#### Javaconfig and/or xml defined beans to xml####

* xml can use beans from another xml by adding the following element:
    ``<import resource="config.xml" />``

* xml can use beans from javaconfig by simply registering javaconfig class as a bean in xml:
    ``<bean class="com.company.Config" />``

* To add autowired beans to xml, add ``@ComponentScan`` to a javaconfig class, and import it as a bean into xml.

* Another way of adding autowired beans to xml is `<context:component-scan base-package="a.b" />`

Hey, have a great day! You are awesome.
