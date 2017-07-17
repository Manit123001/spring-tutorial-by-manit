# web-customer-track

test Db Connection
1. set up our Eclipse
2. Add JDBC Driver for MySQL
3. Sanity test... make sure we can connect

# test Connecting to database 
## Setup Dev Environment

1. Copy starter config file
- 1. web.xml and spring config
2. Copy over basic libs
- 1. Jakarta Commons and JSTL
3. Copy latest Spring JAR files [Download lib Spring](https://repo.spring.io/release/org/springframework/spring/)
4. Copy latest Hibernate JAR files [Download Hibernate ORM](http://hibernate.org/orm/)
 
## Build a Database Web App - Spring MVC and Hibernate Project  
![image](https://user-images.githubusercontent.com/11830385/28254293-ba9f9474-6ad5-11e7-81d2-d6dbe97df2c4.png)

![image](https://user-images.githubusercontent.com/11830385/28254366-49d25820-6ad6-11e7-887c-8bdb380c4ca7.png)

### CRUD CReate Read Update Delete
![image](https://user-images.githubusercontent.com/11830385/28254380-72583558-6ad6-11e7-9f50-2cd415e0774f.png)

### Step By Step 
![image](https://user-images.githubusercontent.com/11830385/28254399-b0a9948c-6ad6-11e7-833e-1b8bb7e34d58.png)


### Hibernate Terminology Refresh
Entity Class 
* Java class that is mapped to a database table

![image](https://user-images.githubusercontent.com/11830385/28254439-18548d08-6ad7-11e7-8603-91079951fcbb.png)

![image](https://user-images.githubusercontent.com/11830385/28254447-2c24cb0e-6ad7-11e7-8530-298365307711.png)

![image](https://user-images.githubusercontent.com/11830385/28254459-43e07c70-6ad7-11e7-8407-c9ec0d61bf75.png)


![image](https://user-images.githubusercontent.com/11830385/28254474-69621436-6ad7-11e7-9fbb-c1aadc567d4b.png)

* create package com.manit.spring.entity
1. craete class Customer.java
```
@Entity
@Table(name="customer")
public class Customer {
	
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	@Column(name="id")
	private int id;

	@Column(name="first_name")
	private String firstName;
	
	@Column(name="last_name")
	private String lastName;
	
	@Column(name="email")
	private String email;
 
 .... create get set
}
```

2. create DAO


![image](https://user-images.githubusercontent.com/11830385/28254616-6d88da12-6ad8-11e7-9f5c-02e5bfb1f5dc.png)


![image](https://user-images.githubusercontent.com/11830385/28254632-9be5ad90-6ad8-11e7-9682-ef31c4dd2046.png)

![image](https://user-images.githubusercontent.com/11830385/28254655-c8dca02e-6ad8-11e7-9ef7-24bf8671e66a.png)

- Step1 Define DAO interface
```
public interface CustomerDAO{
 public List<Customer> getCustomers();
}
```

- Step2 Define DAO implementation
```
 public class CustomerDAOImpl implements CustomerDAO {
	// need to inject the session factory
	@Autowired
	private SessionFactory sessionFactory;

	@Override
	public List<Customer> getCustomers() {
		
		return null;
	}
}
```

![image](https://user-images.githubusercontent.com/11830385/28254726-a02ee9a6-6ad9-11e7-8c1d-b4001a02f737.png)


![image](https://user-images.githubusercontent.com/11830385/28254731-b3a5d24c-6ad9-11e7-9f5a-13c9fb858fcb.png)



![image](https://user-images.githubusercontent.com/11830385/28254772-1a1184fe-6ada-11e7-971e-865097cc5720.png)

![image](https://user-images.githubusercontent.com/11830385/28254790-4bd5443a-6ada-11e7-98f0-73831bd58928.png)


![image](https://user-images.githubusercontent.com/11830385/28254803-7ec53170-6ada-11e7-9bba-3aec0195a2f9.png)
```
@Repository
public class CustomerDAOImpl implements CustomerDAO {

	// need to inject the session factory
	@Autowired
	private SessionFactory sessionFactory;

	@Override
	@Transactional
	public List<Customer> getCustomers() {

		// get the current hibernate session
		Session currentSession = sessionFactory.getCurrentSession();

		// create a query
		Query<Customer> theQuery = currentSession.createQuery("from Customer", Customer.class);

		// execute query and get result list
		List<Customer> customers = theQuery.getResultList();

		// return the results
		return customers;
	}

}
```
