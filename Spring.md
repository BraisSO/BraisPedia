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
