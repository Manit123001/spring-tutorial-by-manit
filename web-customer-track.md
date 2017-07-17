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
* CustomerDAO.java
* CustomerDAOImpl.java


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

3. Create CustomerController.java

![image](https://user-images.githubusercontent.com/11830385/28255021-79a7af18-6adc-11e7-9a9f-3ba57870c4ea.png)
### update our Controller
![image](https://user-images.githubusercontent.com/11830385/28255008-6427b2d2-6adc-11e7-8154-1d11016832bd.png)
```
@Controller
@RequestMapping("/customer")
public class CustomerController {

	// need to inject the customer dao
	@Autowired
	private CustomerDAO customerDAO;

	@RequestMapping("/list")
	public String listCustomer(Model theModel) {

		// get customers from the dao
		List<Customer> theCustomers = customerDAO.getCustomers();

		// add the customers to the model
		theModel.addAttribute("customers", theCustomers);

		return "list-customers";
	}
}

```

4. Create JSP View Page
* JSP page: list-customers.jsp

```
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<!DOCTYPE html>
<html>
<head>
<title>List Customers</title>
</head>
<body>
	<div id="wrapper">
		<div id="header">
			<h2>CRM - Customer Relationship Manager</h2>
		</div>
	</div>

	<div id="container">
		<div id="content">
			<!-- add out html table here -->
			<table>
				<tr>
					<th>First Name</th>
					<th>Last Name</th>
					<th>Email</th>
				</tr>

				<!-- loop over and print our customers -->
				<c:forEach var="tempCustomer" items="${customers}">
					<tr>
						<td>${tempCustomer.firstName}</td>
						<td>${tempCustomer.lastName}</td>
						<td>${tempCustomer.email}</td>
					</tr>

				</c:forEach>
			</table>

		</div>
	</div>
</body>
</html>
```


## Making CSS
1. Place CSS in a 'resources' directory
2. Configure spring to serve up ' resources' directory
3. Reference CSS in your JSP

#### 1. Step 1
![image](https://user-images.githubusercontent.com/11830385/28255327-addfba84-6adf-11e7-88e7-6a84da8590ff.png)

#### 2. Step 2
![image](https://user-images.githubusercontent.com/11830385/28255344-da894532-6adf-11e7-96a7-174ef01555df.png)

#### 3. step 3
![image](https://user-images.githubusercontent.com/11830385/28255357-03e1b0ea-6ae0-11e7-9d4b-03db2600cb05.png)

![image](https://user-images.githubusercontent.com/11830385/28255382-43d26a00-6ae0-11e7-9998-a25b770dc16f.png)


