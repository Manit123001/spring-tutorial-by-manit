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

---
## Hibernate Project - Part 3
### @GetMapping and @PostMapping

![image](https://user-images.githubusercontent.com/11830385/28258820-75c11346-6afd-11e7-8137-c1362c06ab14.png)

```
@RequestMapping(path="/processForm", method=RequestMethod.POST)
public String processForm(){
	...
}
```
* This mapping ONLY handles **POST** method
* Any other HTTP REQUEST method will get rejected

### New Annotation Short-Cut
```
@PostMapping("/processForm")
public String processForm(...){
	...
}

```
* New annotation: @PostMapping
* This mapping ONLY handles POST method
* Any other HTTP REQUEST method will get rejected

![image](https://user-images.githubusercontent.com/11830385/28259100-d64129a8-6afe-11e7-8287-ad276fa8cc74.png)

### Recap : New Annotations
* @GetMapping
* @PostMapping

### Use New Annotation: @GetMapping

* Refactor : Change to use @GetMapping
![image](https://user-images.githubusercontent.com/11830385/28260136-df5917bc-6b03-11e7-89f0-0ebef15bda72.png)

![image](https://user-images.githubusercontent.com/11830385/28254293-ba9f9474-6ad5-11e7-81d2-d6dbe97df2c4.png)

#service
![image](https://user-images.githubusercontent.com/11830385/28260376-ff361516-6b04-11e7-8967-76944a81285d.png)
![image](https://user-images.githubusercontent.com/11830385/28260435-3ec11dfc-6b05-11e7-911f-4cf270fbe871.png)

![image](https://user-images.githubusercontent.com/11830385/28260632-2a217c1a-6b06-11e7-9bfd-ad4c7218c17b.png)


![image](https://user-images.githubusercontent.com/11830385/28260682-7278ef34-6b06-11e7-823b-fa2ae0eba89b.png)

### Specialized Annotation for Services
* @service applied to Service implementations
* Spring will automatically register the Service implementation
* thanks to component-scanning


![image](https://user-images.githubusercontent.com/11830385/28260832-fe6a38a4-6b06-11e7-909e-8173798523c6.png)
#### Step 1: Define Service interface
```
public interface CustomerService{
	public List<Customer> getCustomer();
}
```
#### Step 2: Define Service implementation
```
@Service
public class Customer ServiceImpl implements CustomerService{
	@Autowired
	private CustomerDAO customerDAO;
	
	@Transactional
	public List<Customer> geCustomer(){
	...
	}
}
```
![image](https://user-images.githubusercontent.com/11830385/28262194-60bce89e-6b0c-11e7-88b0-2483a82d069e.png)

![image](https://user-images.githubusercontent.com/11830385/28262201-6867212c-6b0c-11e7-9c28-2a7e4351462d.png)

####  Step 1 Create package for our Service
* 1. com.manit.spring.service
* 2. Create Service interface (CustomerService.java)
```
public interface CustomerService {	
	public List<Customer> getCustomers();
}
```
* 3. Create Service implementation (CustomerServiceImpl.java) Implement CustomerService.java
```
@Service
public class CustomerServiceImpl implements CustomerService {

	// need to inject customer dao
	@Autowired
	private CustomerDAO customerDAO;
	
	@Override
	@Transactional
	public List<Customer> getCustomers() {
		return customerDAO.getCustomers();
	}

}
```
* 4. Update DAO implementation - remove @Transactional 
(CustomerDAOImpl.java)


![image](https://user-images.githubusercontent.com/11830385/28262816-af040ef4-6b0e-11e7-9e84-463033e620c0.png)

* 5. Update **Controller** - inject CustomerService
![image](https://user-images.githubusercontent.com/11830385/28262877-f081e7e8-6b0e-11e7-8446-f9c9d206544c.png)
```
@Controller
@RequestMapping("/customer")
public class CustomerController {

	// need to injecdt our customer service
	@Autowired
	private CustomerService customerService;
	
	@GetMapping("/list")
	public String listCustomer(Model theModel) {

		// get customers from the dao
		List<Customer> theCustomers = customerService.getCustomers();

		// add the customers to the model
		theModel.addAttribute("customers", theCustomers);

		return "list-customers";
	}
	
}

```

** Delegate calls to Service
![image](https://user-images.githubusercontent.com/11830385/28263449-012f773e-6b11-11e7-998f-314d19a0e750.png)

---
## Hibernate Project - Part 4

![image](https://user-images.githubusercontent.com/11830385/28304958-1c367768-6bc4-11e7-9c60-a9cb8ef76433.png)

![image](https://user-images.githubusercontent.com/11830385/28305034-5b39c758-6bc4-11e7-9067-9c1d9357b531.png)

1. Update list-customer.jsp

* Add Customer Button
<!-- put new button: Add Customer -->
<input type="button" value="Add Customer"
	onclick="window.location.href='showFormForAdd'; return false;"
	class="add-button"
/>

---

![image](https://user-images.githubusercontent.com/11830385/28305034-5b39c758-6bc4-11e7-9067-9c1d9357b531.png)
![image](https://user-images.githubusercontent.com/11830385/28306482-a72a94ee-6bc9-11e7-87d1-c2676d2c0e11.png)


2. Create HTML form for new customer
* add code to CustomerController.java
```
	@GetMapping("/showFormForAdd")
	public String showFormForAdd(Model theModel) {

		return "customer-form";
	}
```
* new form
```
	@GetMapping("/showFormForAdd")
	public String showFormForAdd(Model theModel) {

		// creat model attribute to bind form data
		Customer theCustomer = new Customer();
		
		theModel.addAttribute("customer", theCustomer);
		
		return "customer-form";
	}
```

![image](https://user-images.githubusercontent.com/11830385/28307077-d7a8215c-6bcb-11e7-86b2-02c4b58df7d2.png)

* Let's create customer-form.jsp
```
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
```
* link css
```
	<link type="text/css"
		rel="stylesheet"
		href="${pageContext.request.contextPath}/resources/css/style.css">

	<link type="text/css"
		rel="stylesheet"
		href="${pageContext.request.contextPath}/resources/css/add-customer-style.css">
	
```

* form of spring
```
	<form:form action="saveCustomer" modelAttribute="customer" method="POST">
			<table>
				<tbody>
					<tr>
						<td><label>First name:</label></td>
						<td><form:input path="firstName"/></td>
					</tr>
					
					<tr>
						<td><label>Last name:</label></td>
						<td><form:input path="lastName" /></td>
					</tr>
					
					<tr>
						<td><label>Email:</label></td>
						<td><form:input path="email"/></td>
					</tr>
					
					<tr>
						<td><label></label></td>
						<td><input type="submit" value="Save" class="save"/></td>
					</tr>
					
				</tbody>
			</table>
		</form:form>
		<div style="clear; both;"></div>
		<p>
			<a href="${pageContext.request.contextPath}/customer/list">Back to List</a>
		</p>

```

![image](https://user-images.githubusercontent.com/11830385/28307381-eb7be4e2-6bcc-11e7-89e9-4d88d51a0e58.png)

![image](https://user-images.githubusercontent.com/11830385/28300880-9d98053c-6bac-11e7-9f1d-d9a5cdeb8f4c.png)




3. Process Form Data 
* Controller -> Service -> DAO
* Review our HTML form: customer-form.jsp
```
	@PostMapping("/saveCustomer")
	public  String saveCustomer(@ModelAttribute("customer") Customer theCustomer){
	
		
		// save the customer using our service
		customerService.saveCustomer(theCustomer);
		
		
		return "redirect:/customer/list";
	}
```

![image](https://user-images.githubusercontent.com/11830385/28307679-d82dea92-6bcd-11e7-95f3-273e72153105.png)

* auto create interface **saveCustommer(theCustomer);** on service
* create method intype on CustomerDAO 
* add implement saveCustommer(Customer theCustomer) methods
* on method in CustomerDAO.java
* get current hibernate session
```
	@Override
	public void saveCustomer(Customer theCustomer) {
		// get current hibernate session
		Session currentSession = sessionFactory.getCurrentSession();
		// save the customer ... finally LOL
		currentSession.save(theCustomer);
	}
```

![image](https://user-images.githubusercontent.com/11830385/28308102-62fd49b4-6bcf-11e7-9ffe-c103534c307a.png)

![image](https://user-images.githubusercontent.com/11830385/28309084-6e6c32d0-6bd2-11e7-8184-8a5f4cd7725c.png)

