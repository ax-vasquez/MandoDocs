# [Microservices with Spring](https://spring.io/blog/2015/07/14/microservices-with-spring)
- This article covers how to create a simple microservices application using:
  - Spring
  - Spring Boot
  - Spring Cloud

The source article aims to clarify how things work by building the simplest possible system, step-by-step. _The writer only implements a small part of the big system_ - the "user account service".

## What are Microservices?
A _microservice_ is a stand-alone process in a larger application that handles a well-defined requirement. They allow large systems to be built up from a number of collaborating components.
- It does at the _process level_ what Spring has always done at the component level:
  - _Loosely-coupled processes instead of loosely coupled components_

## Back to the Demonstration
This project, _Web-Application_, will make requests to the _Account-Service_ microservice using a RESTful API. We will also need to add a _discovery_ service so that the processes can find each other.

### Service Registration
- When you have multiple processes working together, they need to be able to find each other
  - If you have ever used Java RMI (Remote Method Invocation) mechanism, recall that it relied on a central registry
  - Microservices have the same requirement
- Netflix created a registration server called Eureka
  - They made this open-source and it has since been incorporated into Spring Cloud
  - This makes it even easier to set up a Eureka server

_**The following is the COMPLETE discovery-server application**_:
```java
@SpringBootApplication
@EnableEurekaServer
public class ServiceRegistrationServer {

  public static void main(String[] args) {
    // Tell Boot to look for registration-server.yml
    System.setProperty("spring.config.name", "registration-server");
    SpringApplication.run(ServiceRegistrationServer.class, args);
  }
}
```
_It really is that simple!_

### Spring Cloud `pom.xml`
Spring Cloud is built on Spring Boot and utilizes parent and starter POMs. The important parts of the POM are:
```xml
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.10.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <!-- Setup Spring Boot -->
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <!-- Setup Spring MVC & REST, use Embedded Tomcat -->
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <!-- Spring Cloud starter -->
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter</artifactId>
        </dependency>

        <dependency>
            <!-- Eureka for service registration -->
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka-server</artifactId>
        </dependency>
    </dependencies>

   <!-- Spring Cloud dependencies -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Edgware.SR3</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```
- By default, Spring applications look for an `application.properties` or `application.yml` file for configuration
  - By setting `spring.config.name` property, you can tell Spring Boot to look for a different file
    - This is very useful if you have multiple Spring Boot applications in the same object (as demonstrated in this project later)

#### Discovery Server Configuration
This application looks for `registration-server.properties` or `registrations-server.yml`. _Here is the relevant configuration from `registration-server.yml`_
```yml
# Configure this Discovery Server
eureka:
  instance:
    hostname: localhost
  client:  # Not a client, don't register with yourself (unless running
           # multiple discovery servers for redundancy)
    registerWithEureka: false
    fetchRegistry: false

server:
  port: 1111   # HTTP (Tomcat) port
```
- By default, Eureka runs on port `8761`, but this configuration file sets it to `1111`
  - By including the registration code in your process, the process can be either a client or server
  - This configuration file specifies the given process as a _server_ (not a client)
    - This prevents the server process from trying to register with itself

### Creating a Microservice: _Account-Service_
_NOTE: Going forward, the "Eureka" server will be referred to as the **discovery-server** since you don't have to use Eureka - Spring also supports the Consul registry service - both will work fine from this point on._ 

When configuring applications with Spring, _emphasize **Loose Coupling and Tight Cohesion**_.

In this example, there is a simple Account management microservice that uses Spring Data to implement a JPA `AccountRepository` and Spring REST to provide a RESTful interface to account information. _In most respects, this is a straightforward Spring Boot application_. What makes it special is that is _registers itself with the discovery-server at startup_. Here is the Spring Boot startup class:
```java
@EnableAutoConfiguration
@EnableDiscoveryClient
@Import(AccountsWebApplication.class)
public class AccountsServer {

    @Autowired
    AccountRepository accountRepository;

    public static void main(String[] args) {
        // Will configure using accounts-server.yml
        System.setProperty("spring.config.name", "accounts-server");

        SpringApplication.run(AccountsServer.class, args);
    }
}
```
The annotations do the work:
1. `@EnableAutoConfiguration`
    - Defines this as a Spring Boot application
2. `@EnableDiscoveryClient`
    - Enables service registration and discovery
    - In this case, this process registers itself with the _discovery-server_ service using its application name
3. `@Import( AccountsWebApplication.class )`
    - This Java configuration class sets up everything else

**_What makes this a MICROSERVICE is the registration with the discovery-server via `@EnableDiscoveryClient` and its YML configuration completes the setup_**:
```yml
# Spring properties
spring:
  application:
     name: accounts-service

# Discovery Server Access
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:1111/eureka/

# HTTP Server
server:
  port: 2222   # HTTP (Tomcat) port
```
- Note that this YML file:
  - Sets the application name as `accounts-service`
    - This service registers under this name **_and can also be accessed by this name_**
  - Specifies a custom port to listen on (`2222`)
    - Processes cannot share ports within the same discovery-service, so it's important to differentiate the port numbers
  - Specifies the URL of the Eureka Service process from the previous section

#### Configuration Option - Registration Time
Registration takes up to 30 seconds because that is the default client refresh time. This can be changed by setting the property `eureka.instance.leaseRenewalIntervalInSeconds` to a different value. For example:
```yml
eureka:
  instance:
    leaseRenewalIntervalInSeconds: 5         # DO NOT DO THIS IN PRODUCTION
```

#### Configuration Option - Registration ID
A process (microservice) registers with the discovery-service using a unique id.
- If another process registers with the _same ID_, **it is treated as a restart** and the first process registration is discarded
  - This gives us the fault-tolerant system we desire
- To run multiple instances of the _same_ process:
  - They each need to register with a unique ID
  - This is handled _automatically_ in the `Brixton` release train

Under the `Angel` release train, this _was not handled automatically_, though you could still achieve the same result by setting the ID property manually via the client's Eureka Metadata map like so:
```yml
eureka:
  instance:
    metadataMap:
      instanceId: ${spring.application.name}:${spring.application.instance_id:${server.port}}
```
_But what does this do?_
- We set `instanceId` to `application-name:instance_id`
  - But if the `instance_id` is not defined, we will use `application-name::server-port` instead
- Note that the `spring.application.instance_id` is _only_ set when using Cloud Foundry
  - It conveniently provides a unique ID number for each instance of the same application

**NOTE**: The syntax `${x:${y}}` is Spring property shorthand for `${x} != null ? ${x} : ${y}`

Since the `Brixton` release, there is also a dedicated property for this:
```yml
eureka:
  instance:
    instanceId: ${spring.application.name}:${spring.application.instance_id:${server.port}}
```

## Accessing the Microservice: _Web-Service_
To consume a RESTful service, Spring provides the `RestTemplate` class.
- This class allows you send HTTP requests to a RESTful server and fetch data in a number of formats such as JSON and XML
  - Which formats can be used is dependent on the presence of marshaling classes on the classpath
    - For example, JAXB is always detected since it's a standard part of Java
    - JSON is supported if Jackson jars are present in the classpath

A microservice (discovery) client can use a `RestTemplate` and Spring will automatically configure it to be microservice aware.

### Encapsulating Microservice Access
Here is part of the `WebAccountService` for the sample client application:
```java
@Service
public class WebAccountsService {

    @Autowired        // NO LONGER auto-created by Spring Cloud (see below)
    @LoadBalanced     // Explicitly request the load-balanced template
                      // with Ribbon built-in
    protected RestTemplate restTemplate; 

    protected String serviceUrl;

    public WebAccountsService(String serviceUrl) {
        this.serviceUrl = serviceUrl.startsWith("http") ?
               serviceUrl : "http://" + serviceUrl;
    }

    public Account getByNumber(String accountNumber) {
        Account account = restTemplate.getForObject(serviceUrl
                + "/accounts/{number}", Account.class, accountNumber);

        if (account == null)
            throw new AccountNotFoundException(accountNumber);
        else
            return account;
    }
    ...
}
```
- Note that `WebAccountService` is just a wrapper for the `RestTemplate` fetching data from the microservice
  - The interesting parts are `serviceUrl` and `RestTemplate`

### Accessing the Microservice
As shown below, the `serviceUrl` is provided by the main program to the `WebAccountController` (which in turn passes it to the `WebAccountService`)
```java
@SpringBootApplication
@EnableDiscoveryClient
@ComponentScan(useDefaultFilters=false)  // Disable component scanner
public class WebServer {

    // Case insensitive: could also use: http://accounts-service
    public static final String ACCOUNTS_SERVICE_URL
                                        = "http://ACCOUNTS-SERVICE";

    public static void main(String[] args) {
        // Will configure using web-server.yml
        System.setProperty("spring.config.name", "web-server");
        SpringApplication.run(WebServer.class, args);
    }

    @LoadBalanced    // Make sure to create the load-balanced template
    @Bean
    RestTemplate restTemplate() {
        return new RestTemplate();
    }

    /**
     * Account service calls microservice internally using provided URL.
     */
    @Bean
    public WebAccountsService accountsService() {
        return new WebAccountsService(ACCOUNTS_SERVICE_URL);
    }

    @Bean
    public WebAccountsController accountsController() {
         return new WebAccountsController
                       (accountsService());  // plug in account-service
    }
}
```
- Some notes on this
  - The `WebController` is a typical Spring MVC view-based controller returning HTML
    - The application uses Thymeleaf as the view-technology (for generating dynamic HTML)
  - `WebServer` is also a `@EnableDiscoveryClient`
    - In this case, as well as registering itself with the discovery-server (which is not necessary since it offers no services of its own) it uses Eureka to locate the account service
  - The default component-scanner setup inherited from Spring Boot looks for `@Component` classes and, in this case, finds my `WebAccountController` and tries to create it
    - In the example above, the component scanner is disabled, as indicated by `@ComponentScan( useDefaultFilters = false )`
  - The _service-url_ being passed to the `WebAccountController` is the name the service used to register itself with the _discovery-server_
    - By default, this is the same as `spring.application.name` for the process which is account-service
    - The user of upper-case _is not required_, but does help to emphasize that ACCOUNTS-SERVICE is a logical host (that will be obtained via discovery)
      - It IS NOT an actual host

### Load Balanced RestTemplate
The `RestTemplate` bean will be intercepted and auto-configured by Spring Cloud (due to the `@LoadBalanced` annotation) to use a custom `HttpRequestClient` that uses Netflix Ribbon to do the microservice lookup.
- Ribbon is also a load-balancer, so if you have multiple instances of a service available, it picks one for you
  - Netflix Eureka and Consul don't offer load balancing on their own - this is why Ribbon is needed

_**NOTE: From the `Brixton` release train, the `RestTemplate` is no longer created automatically.** It used to be created for you, which caused confusion and potential conflicts_.

The following instance is qualified using `@LoadBalanced`. If you have more than one `RestTemplate` bean, you can make sure to inject the right one like so:
```java
    @Autowired
    @LoadBalanced     // Make sure to inject the load-balanced template
    protected RestTemplate restTemplate;
```
- A `RestTemplate` instance is thread-safe
  - It can be used to access any number of services in different parts of your application

### Configuration
- The following depicts the relevant configuration from `web-server.yml` - it is used to:
  - _Set the application name_
  - _Define the URL for accessing the discovery server_
  - _Set the Tomcat port to `3333`_
```yml
# Spring Properties
spring:
  application:
     name: web-service

# Discovery Server Access
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:1111/eureka/

# HTTP Server
server:
  port: 3333   # HTTP (Tomcat) port
```

## Extra Notes
If you are not familiar with Spring Boot, this section explains some of the "magic"

### View Templating Engines
The Eureka dashboard (inside `RegistrationServer`) is implemented using FreeMarker templates, but the other two applications use Thymeleaf.
- To make sure each uses the right view engine, there is extra configuration in each YML file
- The following is at the end of `registration-server.yml` to disable Thymeleaf:
```yml
...
# Discovery Server Dashboard uses FreeMarker.  Don't want Thymeleaf templates
spring:
  thymeleaf:
    enabled: false     # Disable Thymeleaf spring:
```

- Since both `AccountService` and `WebService` use thymeleaf, we also need to _point each at their own templates_
  - Here is part of `account-server.yml`:
```yml
# Spring properties
spring:
  application:
     name: accounts-service  # Service registers under this name
  freemarker:
    enabled: false      # Ignore Eureka dashboard FreeMarker templates
  thymeleaf:
    cache: false        # Allow Thymeleaf templates to be reloaded at runtime
    prefix: classpath:/accounts-server/templates/
                        # Template location for this application only
...
```
- `web-server.yml` is similar, but its templates are defined by:
```yml
   prefix: classpath:/web-server/templates/
```

**IMPORTANT!!! _Note the `/` on the end of each `spring.thymeleaf.prefix` classpath - THIS IS CRUCIAL!!!_**

### Command-Line Execution
The sample code jar is compiled to automatically run `io.pivotal.microservices.services.Main` when invoked from the command-line. See [Main.java](https://github.com/paulc4/microservices-demo/blob/master/src/main/java/io/pivotal/microservices/services/Main.java)
- The Spring Boot option to set the `start-class` can be seen in the POM:
```xml
    <properties>
        <!-- Stand-alone RESTFul application for testing only -->
        <start-class>io.pivotal.microservices.services.Main</start-class>
    </properties>
```

### `AccountsConfiguration` Class
This is the main configuration class for `AccountService`, which is a classic Spring Boot application using Spring Data.
```java
@SpringBootApplication
@EntityScan("io.pivotal.microservices.accounts")
@EnableJpaRepositories("io.pivotal.microservices.accounts")
@PropertySource("classpath:db-config.properties")
public class AccountsWebApplication {
...
}
```
The annotations do most of the work:
1. `@SpringBootApplication`
    - Defines this as a Spring Boot application
    - It combines:
      - `@EnableAutoConfiguration`
      - `@Configuration`
      - `@ComponentScan`
        - This, by default, causes Spring to search the package containing this class, and its sub-packages, for potential Spring Beans
2. `@EntityScan("io.pivotal.microservices.accounts")`
    - Since this project uses JPA, it needs to specify where the `@Entity` classes are
      - Normally this is an option you specify in JPA's `persistence.xml` or when creating a `LocalContainerEntityManagerFactoryBean`
      - Spring Boot will create this factory bean because the `spring-boot-starter-data-jpa` dependency is on the class path
3. `@EnableJpaRepositories("io.pivotal.microservices.accounts")`
    - Look for classes extending Spring Data's `Repository` marker interface and automatically implement them using JPA
4. `@PropertySource("classpath:db-config.properties")`
    - Properties to configure the `DataSource`
      - See [db-config.properties](https://github.com/paulc4/microservices-demo/blob/master/src/main/resources/db-config.properties)

### Configuring Properties
Spring Boot applications look for either `application.properties` or `application.yml` to configure themselves. _Since all three servers used in the sample application are in the same project, they would automatically use the same configuration._

To avoid this, each specifies an alternative file by setting the `spring.config.name` property. For example, here is _part of_ `WebServer.java`:
```java
public static void main(String[] args) {
  // Tell server to look for web-server.properties or web-server.yml
  System.setProperty("spring.config.name", "web-server");
  SpringApplication.run(WebServer.class, args);
}
```
- At runtime, the application will find and use `web-server.yml` in `src/main/resources`

### Logging
Spring sets up `INFO` level logging for Spring by default. You can raise the level of logging to `WARN` to reduce the amount of logging.
- To do this, the logging level would need to be specified in each of the configuration files
  - This is usually the best place to define them as logging properties _cannot_ be specified in property files
    - Logging has already been initialized before `@PropertySource` directives are processed
