## Hello World with Spring Security

This is an example of a minimal implementation of security using the Spring Security framework.

The application has the following dependencies, which can be built with either Maven or Gradle:

- Spring Web
- Spring Security

The Maven dependencies are listed below:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

In this project, we use basic authentication, which transfers credentials as username:password pairs encoded in Base64. This method is not secure for protecting requests but is used here to simplify the example.

After creating the project, we add a controller with an endpoint that the client can access to return a simple message such as "Hello, world!".

```java
// Controller.java
@RestController
public class Controller {
    @GetMapping("/hello")
    public String hello() {
        return "Hello there!";
    }
}
```

By default, Spring Boot points to the location "http://localhost:8080" if no configuration is specified. You can access it using the cURL command:

```powershell
curl http://localhost:8080
```

The response will be "Hello there!".

The security layer is implemented using two classes: UserDetailsService and PasswordEncoder. The UserDetailsService retrieves the user details, which is an object of a class implementing the UserDetails interface, and stores the user information. The PasswordEncoder bean is responsible for hashing the password before comparing it to the stored value. In this example, they are implemented as beans in the class annotated with @Configuration.

```java
// ProjectConfig.java
@Configuration
public class ProjectConfig {
    @Bean
    public UserDetailsService userDetailsService() {
        UserDetails user = User.withUsername("john")
            .password("12345")
            .build();

        return new InMemoryUserDetailsManager(user);
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return NoOpPasswordEncoder.getInstance();
    }
}
```

After that, the endpoint is secured and can be accessed using cURL:

```
curl.exe -u "john:12345" "http://localhost:8080"
```