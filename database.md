#Database

As a developer, I would like to think that databases solve one problem: persistence. I like to work with objects, and with most programs being long running processes, we can run into a situation where we create so many objects that we run out of memory. Another motivation, is that we want our object to outlive our application. That is, if the program is stopped, we want to have access to the same data when it is restarted. Whatever the reason, databases are a very common solution available.

So what do we want to store exactly?
Objects have 2 types of members: fields and methods.
Class files take care of keeping records of methods. The fields, are stored in the database.

The most common way, is to think about all the fields an object has, and to come up with a table that maps directly to those fields:

```
private String name;
private String lastName;
private String phone;
```
maps to a table:

```
name | lastName | phone
------------------------------
John | Bergerman| 555-555-5555
```

In this case, we have one-to-one correspondence between an object of that class, and a row in the table. What this means, is that every object in our program has a record in the database in a form of a row. Likewise, every row in the database can have an object in the program.

When we need to load the information from that table, we can build an object and populate the fields from the information in the table.

The question then arises, how exactly do we communicate with the database? Database is a program that runs on a computer that is very likely to be connected to the internet. We need to know the URL or IP of the database, and login and password given to us by the database administrator or provider. Almost all relational databases speak SQL, or some dialect of it.
SQL is a language that we can use to communicate with the database, to store and retrieve information.

The most basic way to handle our imaginary situation above, is to have some sort of a table that will hold objects of the class that we have. What should this table be called? Common convention is to call it the same as the class, whose instances it will be storing!
If we have a class called Client, we can have a table called client. This is just a convention, and you can use any name that makes sense, as long as it is meaningful to you and the team.

Moving along.
So we have a table and a program that wants to store objects in that table. The objects should all belong to the same class, Client.

We can run a query that gets all of the data needed for one Client object. To know which row stores information about which client, we add a field called `id`. It uniquely identifies both a row in the database and an object in our program. What this means, is that we should keep track of this number, and every time we create a new object of the class Client, we increment the `id` and store this new value in an object's field.
Our table and class now look like this:
```
public class Client{
private long id;
private String name;
private String lastName;
private String phone;
```


```
Table client:
id | name | lastName | phone
-----------------------------------
1  | John | Bergerman| 555-555-5555
-----------------------------------
2  | Anna | Bergerman| 555-555-5556
```

What we can do now, is think about having some sort of a class that sits between our program and database and uses both SQL and, for example, Java to move data between our database and our program. What would it do?

The most obvious thing, is that it should be able to take one object and store it in the database.

Say we have Client instance that we gave the following values:
```
private long id = 1;
private String name = "John";
private String lastName = "Bergerman";
private String phone = "555-555-5555";

```
To store this object, the following SQL statement would need to be run:

```
INSERT INTO client VALUES( 1, John, Bergerman, 555-55-5555 );
```
Please note, that this syntax assumes that the order of the values provided in SQL matches the order defined in the table: id, name, lastName, phone.

So far we talked about storing our objects in the database, how do we get them back?

Let's review our simple structure:

Java | something connecting Java and database | SQL database

Our Java program, should take an object and give it to the intermediary, who will in turn insert the information from this object into the database using SQL.

When time comes, we may need to see this object again. We can take an id of the object, in this case it is 1, and give it our intermediary, and ask it to give us our object from the database. The intermediary needs to know what type(class) of the object to return. It then goes into the corresponding table (Client.java - client table) and finds a row that has an id given by the Java program, it then runs the following SQL:
```
SELECT * FROM client WHERE id=1;
```
Note that we provided both table name and id for this query.

The result of this request, is one row from the database that contains all of the needed pieces to recreate a Java object.
```
1  | John | Bergerman| 555-555-5555
```
into
```
Client client = new Client():
client.setId(1);
client.setName("John");
client.setLastName("Bergerman");
client.setPhone("555-55-5555");
```

Here is the review of what is happening:
1) Java program takes one of the objects and passes it to the intermediary.
2) Intermediary looks at the values in the fields of the object and it's class and runs `insert` statement to store this information.

Time goes by...

3) Java program is ready to retrieve the object back from the database and gives our intermediary an id.
4) Intermediary uses the value given to it to run `select` SQL to get a row with information needed to repopulate the object, and recreates the object.

We now have a basic way to store and retrieve object's data in the database using SQL.
