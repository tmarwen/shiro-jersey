Apache Shiro Jersey Extension
============

Extension for Apache Shiro to work with [Sun Jersey] (http://jersey.java.net "Sun Jersey"). This extension enables Shiro annotations in the JAX-RS implementation [Sun Jersey] (http://jersey.java.net "Sun Jersey"). 

If the user is not authenticated Http Status Code 401 is returned. If the user has insufficient privileges Status Code 403 is returned. This extension provides security using Apache Shiro 
similar to the standard JAX-WS-RS security for Jersey.

The following Shiro Annotations are supported:
+ RequiresPermissions
+ RequiresRoles (combined with AND only)

Examples
-------------

### Checking Permissions with @RequiresPermissions ######
```java
@Path("/changelog") 
@RequiresPermissions("repository:read") 
public class ChangelogResourceImpl { 

   @POST 
   @Consumes(MediaType.APPLICATION_JSON) 
   @Path("/addObject") 
   @RequiresPermissions("repository:write") 
   public Response addObject(ObjectJson objectJson) { 
      someService.addObject(object); 
      return Response.ok().build(); 
   }

} 
```

### Checking Roles with @RequiresRoles ######
```java
@Path("/changelog") 
@RequiresRoles("Admin") 
public class ChangelogResourceImpl { 

   @POST 
   @Consumes(MediaType.APPLICATION_JSON) 
   @Path("/addObject") 
   @RequiresRoles("Chief") 
   public Response addObject(ObjectJson objectJson) { 
      someService.addObject(object); 
      return Response.ok().build(); 
   }

} 
```

Sample Project
-------------
A sample project is provided in the directory jersey-sample. The sample project contains
a Jersey application with REST resources protected by the Jersey extension for Shiro. The 
sample project uses [HTTP Basic](http://en.wikipedia.org/wiki/Basic_access_authentication) 
authentication.

### Users ######
+ root/secret with role admin
+ guest/guest with role guest

### Resources ######
#### Hello Resource ######
The hello resource takes one parameter and displays a plain text message with the given
parameter.

[http://localhost:9080/api/hello/buddy](http://localhost:9080/api/hello/buddy)

#### Admin Resource ######
The admin resource can only be accessed by users with role admin.

[http://localhost:9080/api/admin](http://localhost:9080/api/admin)

### Running the Sample Project ######
You can run the sample project either using maven with the command:

    maven jetty:run

You can also use the Eclipse Web Tooling to run the sample project from Eclipse.

Getting Started
-------------
Using the Jersey Extension for Shiro is pretty easy. Just add an init parameter to 
the configuration of your Jersey servlet as shown below.

```xml
<servlet>
	<servlet-name>jersey-servlet</servlet-name>
	<servlet-class>com.sun.jersey.spi.container.servlet.ServletContainer</servlet-class>
	<init-param>
		<param-name>com.sun.jersey.config.property.packages</param-name>
		<param-value>org.apache.shiro.jersey.sample</param-value>
	</init-param>
    <init-param>
        <param-name>com.sun.jersey.spi.container.ResourceFilters</param-name>
        <param-value>org.apache.shiro.jersey.ShiroResourceFilterFactory</param-value>
    </init-param>
	<load-on-startup>1</load-on-startup>
</servlet>
```

OSGi Support
-------------
The Apache Shiro Jersey Extension fully supports OSGi and is provided as valid OSGi bundle.