# Spring JDBC

Spring adds another simplification to JDBC in the form of JDBC template.
Template, as used here, means that some common functionality is provided, you just have to add pieces that are unique to your use case.

There is a series of steps that we need to go through to properly configure JDBC in spring.

* Datasource
* JDBCTemplate
* Repository that uses JDBCOperations to perform CRUD operations.


#### Datasource
DataSource is similar to DriverManager, but offers additional functionality:

* Connection pool - speeds up DB access using a set of always-open connections.
*Closing a connection from the pool doesn't close that connection, but places it back into the pool and makes it available for other requests.*

We can configure a pooled DataSource as a bean in the Spring container:

```
@Bean // javaconfig: bean name is 'dataSource', from method name
public DataSource dataSource() {
 DataSource ds = new BasicDataSource();
 ds.setDriverClassName("org.h2.Driver");
 ds.setUrl("jdbc:urlhere");
 ds.setUsername("our_app");
 ds.setPassword("123abc");
 ds.setInitialSize(5);  // initial connection pool size
 ds.setMaxActive(10);   // maximum connection pool size
return ds;
}
```


#### JDBCTemplate in repository

According to the domain driven design, we provide repository for our object, and repository is responsible for management of underlying persistent mechanism. (Database in this case)

The best approach is to make JDBCTemplate a field in our repository, and make DataSource a required dependency.
We can use DataSource instance to create JdbcTemplate directly.

```
@Repository
public class JDBCMessageRepository implements MessageRepository{

 private JdbcTemplate jdbcTemplate;

 @Autowired
 public JDBCMessageRepository(DataSource dataSource){
  this.jdbcTemplate = new JdbcTemplate(dataSource);
 }
```

#### Using JDBCTemplate in the repository

This method shows how to insert a message:
```
 public void addMessage(Message m) {
  this.jdbcTemplate.update(
    "insert into message values (" + m.getMessage() + ")",
  );
 }

```
**RowMapper** helps us translate each of the rows returned, into the desired object.

**JdbcTemplate simply applies `mapRow` method to each row in the result set.**
```
public List<Message> allMessages(){

 return this.jdbcTemplate.query(
  "select * from message",
  (ResultSet rs, int n)
     -> { return new Quote(
           rs.getString("quote"),
           rs.getString("attributedTo"));}
 );

}

} // class end.
```


Every @Repository is a @Component, and will be added as bean into spring context.

We can inject JDBCMessageRepository into a MessageService object, where it will be used to perform application specific business logic (use cases).
