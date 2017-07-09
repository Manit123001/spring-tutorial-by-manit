# Spring Framwork Basic

## Init Spring
1. Download Spring JAR Files http://repo.spring.io/release/org/springframework/spring/ Select the lastest release
2. Copy all of the JAR files 
3. Paste to folder **lib** 
4. Download Apache Commons Logging Download https://commons.apache.org/proper/commons-logging/
5. commons-logging-1.2.zip then copy the JAR file (commons-logging-1.2.jar) to your project in **lib**
6. then Right-click >> Properties >> Java Build Path >> Add JARs.... >> select all of jar file and then OK

## Inversion of Control (IoC)- XML Configuration
The approach of outsourcing the construction and managementof objects

create AppicationContext.xml
1. Step 1 Configure your Spring Beans
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">

	<!-- load the properties file: sport.properties -->
	<context:property-placeholder location="classpath:sports.properties" />

  <!-- Define your beans here -->

	<!--Step 1 -->
	<bean id="myCoach" class="com.manit.spring.home.basic.TrackCoach">

		<!-- set up constructor injection -->
		<constructor-arg ref="myFortuneService" />
	</bean>


</beans>







```


