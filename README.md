# spring_security
Learning about spring security - login / logout

- In this project we will learn how to add authentication to spring web app.
- We will use maven for creating project.
- We will use pure java configuration (also for web.xml and dispatcher-servlet.xml)

### Setting up the project

#### STEP 1 : Adding war plugin for replacing web.xml file

##### pom.xml

- below dependencies in *build* add following :

    	<build>
    		<finalName>spring-security-demo</finalName>
    
    		<pluginManagement>
    
    			<plugins>
    
    				<plugin>
    
    					<!-- Add Maven coordinates (GAV) for maven-war-plugin as I don't have 
    						web.xml file -->
    
    					<groupId>org.apache.maven.plugins</groupId>
    					<artifactId>maven-war-plugin</artifactId>
    					<version>3.2.0</version>
    				</plugin>
    
    			</plugins>
    
    		</pluginManagement>
    
    	</build>

- adding maven-war-plugin we don't need to create the web.xml file

#### STEP 2 : Adding the mandatory dependencies

  	<dependencies>
  
  		<!-- Spring MVC support -->
  		<dependency>
  			<groupId>org.springframework</groupId>
  			<artifactId>spring-webmvc</artifactId>
  			<version>${springframework.version}</version>
  		</dependency>
  
  
  		<!-- Servlet, JSP and JSTL support -->
  		<dependency>
  			<groupId>javax.servlet</groupId>
  			<artifactId>javax.servlet-api</artifactId>
  			<version>3.1.0</version>
  		</dependency>
  
  		<dependency>
  			<groupId>javax.servlet.jsp</groupId>
  			<artifactId>javax.servlet.jsp-api</artifactId>
  			<version>2.3.1</version>
  		</dependency>
  
  		<dependency>
  			<groupId>javax.servlet</groupId>
  			<artifactId>jstl</artifactId>
  			<version>1.2</version>
  		</dependency>
  
  		<!-- to compensate for java 9+ not including jaxb -->
  		<dependency>
  			<groupId>javax.xml.bind</groupId>
  			<artifactId>jaxb-api</artifactId>
  			<version>2.3.0</version>
  		</dependency>
  
  		<dependency>
  			<groupId>junit</groupId>
  			<artifactId>junit</artifactId>
  			<version>3.8.1</version>
  			<scope>test</scope>
  		</dependency>
  
  	</dependencies>


#### STEP 3 : Making configuration file :

- as we are following pure java configuration we need to make a java class for configuration
- Make a class named *DemoAppConfig* inside the package *com.rohitThebest.springsecurity.demo.config*.

##### DemoAppConfig.java

      package com.rohitThebest.springsecurity.demo.config;
      
      import org.springframework.context.annotation.Bean;
      import org.springframework.context.annotation.ComponentScan;
      import org.springframework.context.annotation.Configuration;
      import org.springframework.web.servlet.ViewResolver;
      import org.springframework.web.servlet.config.annotation.EnableWebMvc;
      import org.springframework.web.servlet.view.InternalResourceViewResolver;
      
      @Configuration
      @EnableWebMvc // similar as <mvc:annotation-driven>
      @ComponentScan(basePackages = "com.rohitThebest.springsecurity.demo")
      public class DemoAppConfig {
      	
      	// define a bean for view resolver
      	
      	@Bean
      	public ViewResolver viewResolver() {
      		
      		InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
      		
      		viewResolver.setPrefix("/WEB-INF/view/");
      		viewResolver.setSuffix(".jsp");
      		
      		return viewResolver;
      	}
      }

#### STEP 4 : Making a java class for initializing dispatcher-servlet

- Instead of dispatcher-servlet.xml file we can initialize the servlet by ectending the *AbstractAnnotationConfigDispatcherServletInitializer* class and overriding the methods.
- So make a class named *MySpringMvcDispatcherServletInitializer* (package com.rohitThebest.springsecurity.demo.config) and extend it with *AbstractAnnotationConfigDispatcherServletInitializer*

##### MySpringMvcDispatcherServletInitializer.java

      package com.rohitThebest.springsecurity.demo.config;
      
      import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;
      
      public class MySpringMvcDispatcherServletInitializer extends 
      	AbstractAnnotationConfigDispatcherServletInitializer {
      
      	@Override
      	protected Class<?>[] getRootConfigClasses() {
      		// TODO Auto-generated method stub
      		return null;
      	}
      
      	@Override
      	protected Class<?>[] getServletConfigClasses() {
      		return new Class[] {DemoAppConfig.class};
      	}
      
      	@Override
      	protected String[] getServletMappings() {
      		
      		return new String[] {"/"};
      	}
      
      }

#### STEP 5 : Make a controller class

- make a clasd *DemoController* in package *com.rohitThebest.springsecurity.demo.controller*

##### DemoController.java

      package com.rohitThebest.springsecurity.demo.controller;
      
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.GetMapping;
      
      @Controller
      public class DemoController {
      	
      	@GetMapping("/")
      	public String showHome() {
      		
      		return "home";
      	}
      }
      
#### STEP 6 : Make a jsp file

- make *home.jsp* inside WEB-INF/view

##### home.jsp

      <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
          pageEncoding="ISO-8859-1"%>
      <!DOCTYPE html>
      <html>
      <head>
      <meta charset="ISO-8859-1">
      <title>Home</title>
      </head>
      <body>
      	
      	<h2>rohitThebest company Home page</h2>
      	
      	<hr>
      	
      	Welcome to the rohitThebest company Home page!!
      	
      </body>
      </html>
      
 #### STEP 7 : Run the application. 
 
 - As of now we cannot see the login form but ya the app should be up and running successfully.
 
 
