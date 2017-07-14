# Hibernate Config Basic

1. Add Hibernate Configuration file 
2. Annotate Java Class
3. Develop Java Code to perform database operrations

---

### Step 1 Add Hibernate Configuration file 

Hebernate >><<JDBC >><< DB

hibernate.cfg.xml Paste in root of "src" directory

```
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration>

    <session-factory>

        <!-- JDBC Database connection settings -->
        <property name="connection.driver_class">com.mysql.jdbc.Driver</property>
        <property name="connection.url">jdbc:mysql://jdbc:mysql://10.100.60.178/new_db_test</property>
        <property name="connection.username">Developer</property>
        <property name="connection.password">password</property>

        <!-- JDBC connection pool settings ... using built-in test pool -->
        <property name="connection.pool_size">1</property>

        <!-- Select our SQL dialect -->
        <property name="dialect">org.hibernate.dialect.MySQLDialect</property>

        <!-- Echo the SQL to stdout -->
        <property name="show_sql">true</property>

		<!-- Set the current session context -->
		<property name="current_session_context_class">thread</property>
 
    </session-factory>

</hibernate-configuration>
```


### Step 2 Annotate Java Class
Java class >> << Hibernate >> << Database Table

2.1 Map class to database table

```
@Entity
@Table(name="student")
public class Student {
  ...
}
```

2.2 Map fields to database columns ** Student.java**

```
@Entity
@Table(name="student")
public class Student {
 
  @Id
  @Column(name="id")
  private int id;
  
  @Column(name="first_name")
  private String firstName;
  
  ...
}

```
- Create Default Constructor
- Generate Constructor using fields.
- Generate Getter and Setter.


### Step 3 Develop Java Code to perform database operations
#### Hibernate CRUD Apps
1. Create objects /
2. Read objects
3. Update objects
4. Delete object

![image](https://user-images.githubusercontent.com/11830385/28202552-1510b1be-68a0-11e7-8f28-defd5f598700.png)


#### 1. Create object
Java Code Set up ** CreateStudentDemo.java **

#### Two key Plays
**Class** = **Desriptioin**
1. **SessionFactory** = Reads the hibernate config file Creates Session objects Heavy-weight object Only create once in your app
2. **Session** = Wraps a JDBC connection Main object used to save/retrieve objects Short-lived object Retrieve from SessionFactory

```
pulic static void main(String[] args){
	SessionFactory factory = new Configuration()
		.configure("hibernate.cf.xml")
		.addAnnotatedClass(Student.class)
		.buildSessionFactory();
		
	Session session = factory.getCurentSession();
	
	Try {
		// now use the session object to save/retrieve Java objects
	} finally{
		factory.close();
	}
		
}
```

* Save a Java Object

```
	Try {
		// create a student object
		Student tempStudent = new Student("Paul", "Wall", "paul@love.com");
		
		// start transaction
		session.beginTransaction();
				
		// save the student
		session.save(tempStudent);
		
		//commit the transaction
		session.getTransaction().commit();
		
	} finally{
		factory.close();
	}
```
* That so insert to database complete.

#### Hibernate Identity - Primary Key
```
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
 	@Column(name="id")
  	private int id;
	
	...
```
* Let MySQL handle the generation AUTO_INCREMENT

#### ID Generation Strategies

![image](https://user-images.githubusercontent.com/11830385/28158594-63cc690e-67e4-11e7-9f2d-c229d513c7da.png)

** @GeneratedValue this code it can insert multi row in database

```
**test auto_increment**
ALTER TABLE new_db_test.student AUTO_INCREMENT=3000
truncate new_db_test.student
```

---

#### 2. Read objects
Read data from data base 

* Retrieving an Object
File **ReadStudentDemo.java , PrimaryKeyDemo.java**

```
//create java object
Student theStudent = new Student("Daffy", "Duck","daff@love.com");

// save it to database
session.save(theStudent);

// now retrieve/read from database using the primary key
Student myStudent = 
	session.get(Student.class, theStudent.getId());
```

* Querying Objects with Hibernate 
File **QueryStudentDemo.java**
- HIBERNATE 5.2+ UPDATE Below
```
Replace
session.createQuery("from Student").list()
With
session.createQuery("from Student").getResultList()
```
Code createQuery
```
// query students: lastName='Doe'
theStudents = session.createQuery("from Student s where s.id='1'").getResultList();

// display the students
System.out.println("\n\nStudents who have last name of Doe");
displayStudents(theStudents);
```

* log4j show log biding parameter

[View Hibernate SQL parameter Value](https://www.udemy.com/spring-hibernate-tutorial/learn/v4/t/lecture/5835894?start=0)

[download here ](http://central.maven.org/maven2/log4j/log4j/1.2.17/log4j-1.2.17.jar)

---

#### 3. Update objects
Update a Student **UpdateStudentDemo.java**

```
int studentId = 1;
Student myStudent = session.get(Student.class, studentId);

// update first name to "Scooby"
myStudent.setFirstName("Scooby");

// commit the transaction
session.getTransaction().commit();

```

**Update email for all students**
```
session
	.createQuery("update Student set email='foo@gmail.com'")
	.executeUpdate();
```

---

#### 4. Deleting object(s)
Delete a Student **DeleteStudentDemo.java**

```
int studentId = 1;

Student myStudent = session.get(Student.class, studentId);

//delete the student 
session.delete(myStudent);

// commit the transaction
session.getTransaction().commit();
```

**Another way of deleting**
```
session
	.createQuery("delete from Student where id = 2")
	.executeUpdate();

```

```

Steps You Must Complete 

Create the database table.
Set up the Hibernate configuration file.
Create a Java class (entity) with Hibernate annotations.
Develop a main application.
Develop code to save objects.
Develop code to retrieve an object by primary key.
Develop code to query objects to find employees for a given company.
Develop code to delete an object by primary key.




```
[Download](http://www.luv2code.com/downloads/udemy-spring-hibernate/solution-practice-activities.zip)


