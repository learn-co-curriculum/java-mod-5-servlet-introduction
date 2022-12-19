# Introduction to Java Servlets

## Learning Goals

- Define Java Servlet
- Define Java Servlet Container
- Define Servlet Life Cycle 

## Introduction

A Java **Servlet** is a class that is often used for developing dynamic web applications.
Typically, a servlet takes an HTTP request from a browser or other HTTP client, generates dynamic content,
and provides an HTTP response back to the browser/client.
For example, we can implement a Servlet to receive input
from a user through an HTML form, query records from a database based on the user input,
and generate a dynamic web page as a response.

## Java Servlets

A Java servlet class must implement the `javax.servlet.Servlet` interface. 
This interface specifies methods to initialize a servlet, process requests, get information of a servlet,
and terminate a servlet instance.

The `Servlet` interface includes the following methods:

- init(...): Initialize the servlet.  
- destroy(...): Terminate the servlet.  
- service(...): Receive HTTP requests and, by default, dispatch them to the appropriate doXXX() methods.  
- getServletConfig() - Returns an object that contains initialization and startup parameters.  
- getServletInfo(...): Retrieve information about the servlet.  

For Web applications, we implement the Servlet interface by extending the `javax.servlet.http.HttpServlet` abstract class. 
For protocol-independent servlets, we extend the `javax.servlet.GenericServlet` class.

The `HttpServlet` class includes the following methods (among others not shown):

- doGet(...): Execute an HTTP GET request.  
- doPost(...): Execute an HTTP POST request.  
- doPut(...): Execute an HTTP PUT request.  
- doDelete(...): Execute an HTTP DELETE request.  

A servlet class that extends `HttpServlet` would override some or all of these methods
based on the application requirements. For example, we might override the `doGet()` method
to accept user input from an HTML form and generate a response based on the form data.

The `service` and various `do...()` methods take two parameters:

1. An object that implements `HttpServletRequest`, which provides information to the
   servlet regarding the HTTP request, such as request parameter names and values.    
2. An object that implements `HttpServletResponse`, which provides HTTP-specific
   functionality to generate a response, such as specifying content length, MIME type,
   and creating an output stream for the content.

## Servlet Container

A servlet class has no static `main()` method.
Therefore, a servlet must execute under the control of another program
called a **Servlet Container**. 

![Java Servlet Container](https://curriculum-content.s3.amazonaws.com/6036/intro-java-servlets/webserver.png)

A servlet container is usually written in Java and is either part of a Web server
or interacts with a Web server. 
The servlet container calls servlet methods and provides services for the servlet.

The most popular servlet container is **Apache Tomcat**, which is free and open source.

## Servlet Life Cycle

![servlet life cycle](https://curriculum-content.s3.amazonaws.com/6036/intro-java-servlets/lifecycle.png)

The servlet container is responsible for managing the servlet through its life cycle.
The servlet container performs the following sequence of steps during the servlet life cycle:

1. Load the servlet class.  
2. Create a single instance of the servlet.  
3. Call the servlet's `init()` method to perform one-time initializations.  
4. Create a new thread and call the servlet's `service()` method within the thread for each HTTP request.  
   - The `service()` method will determine which method (`doGet()`, `doPost()`, etc) to call based on
     the HTTP request type (`GET`, `POST`, etc).   
5. Call the servlet's `destroy()` method.   

Step 1, 2 and 3 are executed once, when the servlet is initially loaded.
By default, the servlet is not loaded until the first request for a URL
corresponding to the servlet is received, although
we can configure the servlet to load when the container starts.

Step 4 is executed multiple times - once for every HTTP request to the servlet.
The servlet container will create a new thread for each request and 
call the `service()` method on the single servlet instance within the thread.
Multi-threading allows a servlet to handle many concurrent user requests.

Step 5 is executed when the servlet container unloads the servlet,
which occurs when the container shuts down or if the container reloads the
whole web application.

## Conclusion

A Java **Servlet** class is used for developing dynamic web applications.
Using Servlets, we can collect and process input from users from web page forms and other input sources,
query records from a database or another source, and generate web pages dynamically.

## Resources

- [javax.servlet.Servlet](https://javaee.github.io/javaee-spec/javadocs/javax/servlet/Servlet.html)  
- [javax.servlet.HttpServlet](https://javaee.github.io/javaee-spec/javadocs/javax/servlet/http/HttpServlet.html)  
- [What is a servlet?](https://docs.oracle.com/javaee/5/tutorial/doc/bnafe.html)       
- [Intro to Servlets](https://www.baeldung.com/intro-to-servlets)     
- [Intro to Servlets and Servlet Containers](https://www.baeldung.com/java-servlets-containers-intro)  
- [Apache Tomcat](https://tomcat.apache.org/)    
