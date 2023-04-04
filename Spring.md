# Controladores de rutas en Spring 
- `@Controller` se utiliza para devolver vistas
- `@RestController` se utiliza para devolver JSON
- `@GetMapping` es para mapear una petición HTTP GET
- `@PostMapping` es para mapear una petición HTTP POST
- `@RequestMapping` es para mapear una petición HTTP a un método específico en un controlador
- `@RequestParam` se utiliza para obtener parámetros de la petición HTTP
- `Model` es un objeto que se utiliza para almacenar datos que se pasarán a la vista
- `model.addAttribute()` se utiliza para añadir un atributo al modelo

# Tymeleaf 
- `th:text` se utiliza para mostrar el valor de una variable
- `th:each` se utiliza para iterar sobre una colección
- `th:action` se utiliza para especificar el controlador al que se enviará un formulario

# Trabajando con bases de datos en Spring 
- Se puede configurar la conexión a la base de datos en el archivo `application.properties`
- Se suelen crear las siguientes carpetas: `controller`, `model`, `repository`, `service`
- El `Model` es un objeto Java que representa una tabla en la base de datos y tiene las notaciones `@Entity`, `@Table`, `@Id`, `@GeneratedValue`
- El `Repository` es una interfaz que contiene métodos para interactuar con la base de datos y extiende de `JpaRepository`
- El `Service` contiene la lógica de negocio y tiene la notación `@Service`

# Notaciones de relaciones en JPA 
- `@ManyToOne` se utiliza para establecer una relación muchos a uno
- `@OneToMany` se utiliza para establecer una relación uno a muchos

# Notaciones para la salida de JSON 
- `@JsonManagedReference` se utiliza en la relación `@OneToMany`
- `@JsonBackReference` se utiliza en la relación `@ManyToOne`

# Conceptos fundamentales de Spring

**Dependency Injection**
Spring uses dependency injection to manage dependencies between objects. It allows you to easily share objects and services across your application. Here's an example of injecting a service into a controller using the @Autowired annotation:

```java
import org.springframework.beans.factory.annotation.Autowired;

@RestController
public class MyController {

    @Autowired
    private MyService myService;

    @GetMapping("/data")
    public String getData() {
        return myService.getData();
    }
}
```
**Controllers**
Controllers handle incoming HTTP requests and return an HTTP response. They are typically used for tasks such as handling form submissions, handling RESTful web service requests, and rendering views. Here's an example of a simple controller:

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {

    @GetMapping("/greeting")
    public String greeting() {
        return "Hello, World!";
    }
}
```
**Services**
Services are used to share data and functionality across controllers. They are typically used for tasks such as fetching data from a database, performing business logic, and sending email. Here's an example of a simple service:

```java
import org.springframework.stereotype.Service;

@Service
public class MyService {

    public String getData() {
        return "data";
    }
}
```
**Repositories**
Repositories are used to manage data access and provide an abstraction layer between the application and the data storage. They are typically used to work with databases, although they can also be used with other data sources. Here's an example of a simple repository:

```java
import org.springframework.data.repository.CrudRepository;

public interface MyRepository extends CrudRepository<MyEntity, Long> {

}
```
**Spring Boot**
Spring Boot is a framework that makes it easy to create stand-alone, production-grade Spring-based applications. It provides a lot of sensible defaults and makes it easy to configure and run your application. Here's an example of a simple Spring Boot application:

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MyApplication {

    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

**Spring Security**
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
    http.authorizeRequests()
        .antMatchers("/admin/**").hasRole("ADMIN")
        .antMatchers("/user/**").hasRole("USER")
        .anyRequest().permitAll()
        .and()
        .formLogin();
    }
}
```
This code configures the application to require that users have the role "ADMIN" to access the "/admin/" URLs and the role "USER" to access the "/user/" URLs. Any other URLs are open to all users. It also enables form-based authentication, which presents a login page to unauthenticated users.

**Spring Data JPA**
Spring Data JPA is a library that makes it easy to work with JPA (Java Persistence API) in a Spring application. It provides a simple and consistent API for data access, and it also provides support for pagination and sorting. Here's an example of a simple repository interface that extends JpaRepository:

```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface MyRepository extends JpaRepository<MyEntity, Long> {
}
```
**Spring Webflux**
Spring WebFlux is a non-blocking, reactive web framework for building web applications. It is built on top of the Reactor Project, which is a library for building reactive systems. Here's an example of a simple WebFlux controller:

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import reactor.core.publisher.Mono;

@RestController
public class MyController {

    @GetMapping("/greeting")
    public Mono<String> greeting() {
        return Mono.just("Hello, World!");
    }
}
```
It's important to know that Spring Webflux is a different implementation of Spring MVC, and it's focused on providing a non-blocking and reactive programming model.
