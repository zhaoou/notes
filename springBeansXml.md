# Spring notes: beans.xml

While component scan and autowiring are awesome, an xml based configuration is still present in many spring projects.

Like with autowiring and component scan, we need to tell spring which beans to create and how these beans work together.

Once you create beans.xml, inside you can declare beans in the <beans> - root element of the xml.

    <bean class="x.y.Zoo" />

This creates a single bean with id set to *x.y.Zoo#0*, which is a default naming scheme. We can provide the desired id inside of the declaration:

    <bean id="apple" class="x.y.Apple" />

Having declared several beans, we can now wire them, or provide dependencies needed for the application to work:

Spring traditionally provides two types of dependency injection:
**constructor and setter** based injections.

######In **constructor injection** we might want to inject:

Literal values:

    <bean class="x.y.Zoo">
      <constructor-arg value="London zoo" />
    </bean>

References:

    <bean class="x.y.Zoo">
      <constructor-arg value="London zoo" />
    </bean>

Collection of literal values:

    <bean class="x.y.Zoo">
      <constructor-arg>
        <list>
          <value>"zebra"</value>
          <value>"emu"</value>
        </list>
      </constructor-arg>
    </bean>

Collection of references to other beans:

    <bean class="x.y.FruitBasket">
      <constructor-arg>
        <list>
          <ref bean="apple"/>
          <ref bean="pear"/>
        </list>
      </constructor-arg>
    </bean>
Note: instead of lists, we could use maps or sets, or even properties.

######Setter injection:

Just like with the constructor injection, we may want to inject references or literal value (in one example):

    <bean class="x.y.Zoo">
      <property name="type" value="public" />
      <property name="elephant" ref="elephant" />
    </bean>

Collection of literal values:

    <bean class="x.y.Zoo">
      <property name="animals">
        <list>
          <value>elephant</value>
          <value>zebara</value>
        </list>
      </property>
    </bean>

Collection of references:

    <bean class="x.y.Zoo">
      <property name="animals">
        <list>
          <ref bean="elephant" />
          <ref bean="zebara" />
        </list>
      </property>
    </bean>

These are the most important elements of DI using xml in spring.
