## Hello World with Spring Security

This repository provides an example of a minimal implementation of security using the Spring Security framework.

### Dependencies

To build the application, you need the following dependencies, which can be managed with both Maven and Gradle:

- Spring Web
- Spring Security

For Maven, add the following dependencies to your `pom.xml`:

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

### Project Structure

In this project, we use basic authentication, which transfers credentials as username:password pairs encoded in Base64. Please note that this is not a secure way to protect requests, but it is used here for simplicity and illustrative purposes.

After creating the project, we add a controller with an endpoint that the client can access, returning a simple message such as "Hello, world!".

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

### Accessing the Endpoint

By default, the Spring Boot application runs on `http://localhost:8080` if no specific configuration is provided. You can access the endpoint using the cURL command:

```bash
curl http://localhost:8080/hello
```

The response will be "Hello there!".

### Security Configuration

The security layer is implemented using two classes: `UserDetailsService` and `PasswordEncoder`. 

- `UserDetailsService` retrieves the user details and stores the information of the user. It expects an object of a class that implements the `UserDetails` interface.
- `PasswordEncoder` is responsible for hashing the password before comparing it to the stored value.

In this example, these classes are implemented as beans in the `ProjectConfig` class, which is annotated with `@Configuration`.

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

### Securing the Endpoint

After configuring the security layer, the endpoint becomes secured. To access it, you need to provide the correct credentials using cURL:

```bash
curl.exe -u "john:12345" "http://localhost:8080/hello"
```

That's it! You can now explore this example of Spring Security in action. Feel free to customize and build upon it according to your needs.

### References
[1] [Securing a web app](https://spring.io/guides/gs/securing-web/)
[2] [Spring Security in action](https://www.manning.com/books/spring-security-in-action)
