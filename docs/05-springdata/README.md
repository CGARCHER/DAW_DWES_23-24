# SpringData y Bases de Datos en Spring

- [SpringData y Bases de Datos en Spring](#springdata-y-bases-de-datos-en-spring)
  - [Spring Data y Spring Data JPA](#spring-data-y-spring-data-jpa)
  - [Repositorios en Spring Data JPA](#repositorios-en-spring-data-jpa)
    - [Creando consultas para nuestros repositorios](#creando-consultas-para-nuestros-repositorios)
  - [Entidades y Modelos](#entidades-y-modelos)
  - [Relaciones entre entidades](#relaciones-entre-entidades)
  - [Excepciones Personalizadas](#excepciones-personalizadas)
  - [Cargando Datos de Prueba](#cargando-datos-de-prueba)
  - [H2](#h2)
  - [Mysql](#mysql)

## Spring Data y Spring Data JPA

Spring Data es un proyecto dentro del ecosistema de Spring Framework que tiene como objetivo proporcionar una forma más fácil y consistente de interactuar con diferentes tipos de bases de datos desde aplicaciones Java. 

Spring Data ofrece una abstracción de alto nivel sobre las tecnologías de acceso a datos, lo que permite a los desarrolladores centrarse en la lógica de negocio en lugar de preocuparse por los detalles técnicos de la integración con la base de datos. 

Spring Data proporciona soporte para una variedad de tecnologías de acceso a datos, incluyendo bases de datos relacionales, bases de datos NoSQL, bases de datos en memoria y sistemas de archivos. Además, Spring Data proporciona integración con otras tecnologías de Spring, como Spring MVC y Spring Boot, lo que permite a los desarrolladores construir aplicaciones web escalables y fáciles de mantener. 

En resumen, Spring Data es una capa de abstracción que permite a los desarrolladores interactuar con diferentes tipos de bases de datos de una manera coherente y sencilla, reduciendo la complejidad de la integración con la base de datos y acelerando el desarrollo de aplicaciones.

Spring Data JPA es un proyecto dentro del ecosistema de Spring que proporciona una implementación de la API JPA (Java Persistence API) estándar para la persistencia de datos en bases de datos relacionales y se superpone a Hibernate.

JPA es una especificación estándar de Java para el mapeo objeto-relacional (ORM) y proporciona una forma fácil y consistente de interactuar con una base de datos relacional utilizando objetos Java. Spring Data JPA proporciona una capa de abstracción adicional en la parte superior de la API JPA, lo que hace que sea aún más fácil trabajar con bases de datos relacionales que usando Hibernate solamente.

Entre las principales ventajas de Spring Data JPA se encuentran:

- Reduce el código boilerplate: Spring Data JPA elimina gran parte del código repetitivo que normalmente se necesita para interactuar con una base de datos relacional, lo que hace que el código sea más limpio y fácil de mantener.

- Abstracción de bases de datos: Spring Data JPA proporciona una capa de abstracción sobre la base de datos, lo que permite a los desarrolladores escribir código que sea independiente de la base de datos subyacente. Esto significa que puede cambiar fácilmente de una base de datos a otra sin tener que cambiar su código.

- Soporte para repositorios específicos, paginación y ordenamiento: Spring Data JPA proporciona soportes con repositorios específicos, soporte para paginación y ordenamiento de resultados de consultas de forma fácil y sencilla, lo que facilita la implementación de características avanzadas en una aplicación.

- Integración con otras tecnologías de Spring: Spring Data JPA se integra perfectamente con otras tecnologías de Spring, como Spring MVC y Spring Boot, lo que facilita la construcción de aplicaciones web completas.

En resumen, Spring Data JPA proporciona una implementación de la API JPA estándar para la persistencia de datos en bases de datos relacionales y proporciona una capa de abstracción adicional que simplifica el trabajo con bases de datos relacionales en aplicaciones Java.

Para configurar Spring Data JPA en un proyecto, debemos agregar la siguiente dependencia en el archivo pom.xml:

```xml
 <!-- Starter de Spring Data JPA -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

## Repositorios en Spring Data JPA
En Spring Data JPA, un repositorio (anotado con @Repository) es una interfaz que define una colección de métodos para acceder y manipular datos en una base de datos relacional. Un repositorio permite abstraer la capa de acceso a datos y facilita la implementación de operaciones CRUD (crear, leer, actualizar, eliminar) en la aplicación.

Los métodos definidos en un repositorio son anotados con la anotación `@Query` que permite definir consultas personalizadas y personalizar las consultas generadas automáticamente. Además, Spring Data JPA proporciona una implementación predeterminada para los métodos CRUD básicos, lo que significa que no es necesario escribir código adicional para interactuar con la base de datos.

Existen diferentes tipos de repositorios en Spring Data JPA, algunos de los más comunes son:

- JpaRepository: es una interfaz que extiende la interfaz CrudRepository y agrega funcionalidades específicas para JPA. Proporciona operaciones como guardar, eliminar, actualizar y buscar, además de soportar paginación y ordenamiento de los resultados.

- PagingAndSortingRepository: es una interfaz que extiende la interfaz CrudRepository y agrega soporte para paginación y ordenamiento de los resultados.

- CrudRepository: es la interfaz más básica y proporciona las operaciones CRUD básicas, como guardar, eliminar, actualizar y buscar.

- QueryDslPredicateExecutor: es una interfaz que permite utilizar Querydsl para construir consultas dinámicas, lo que permite personalizar las consultas en tiempo de ejecución.

En resumen, un repositorio en Spring Data JPA es una interfaz que define una colección de métodos para acceder y manipular datos en una base de datos relacional. Los repositorios proporcionan una abstracción de la capa de acceso a datos y facilitan la implementación de operaciones CRUD en la aplicación. Existen diferentes tipos de repositorios que ofrecen funcionalidades específicas para JPA y permiten personalizar las consultas en tiempo de ejecución.

### Creando consultas para nuestros repositorios
A la hora de crear consultas para nuestros repositorios, Spring Data JPA nos ofrece diferentes opciones para personalizar las consultas generadas automáticamente y crear consultas personalizadas.

- Consultas personalizadas: Spring Data JPA nos permite crear consultas personalizadas utilizando la anotación @Query. Esta anotación nos permite definir consultas personalizadas utilizando JPQL (Java Persistence Query Language) o SQL nativo. Además, podemos utilizar la anotación @Param para definir parámetros en la consulta.
- Consultas generadas automáticamente: Spring Data JPA [genera automáticamente consultas](https://www.baeldung.com/spring-data-derived-queries) para los métodos definidos en un repositorio. Por defecto, [Spring Data JPA](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repository-query-keywords) genera consultas utilizando el nombre del método y los parámetros definidos en el método. Sin embargo, podemos personalizar las consultas generadas utilizando la anotación @Query.

```java
public interface UserRepository extends JpaRepository<User, Long> {

  // Consulta personalizada utilizando JPQL
  @Query("SELECT u FROM User u WHERE u.email = ?1")
  User findByEmail(String email);

  // Consulta automática utilizando el nombre del método
  Optional<User> findByUsername(String username);

  // Consulta nativa utilizando SQL
  @Query(value = "SELECT * FROM users WHERE phone_number = ?1", nativeQuery = true)
  User findByPhoneNumber(String phoneNumber);
}


```

## Entidades y Modelos
En Spring Data JPA, una entidad es una clase Java que se mapea a una tabla en una base de datos relacional. Para definir una entidad en JPA, se deben seguir los siguientes pasos:

1. Anotar la clase Java con la anotación `@Entity`: Esto indica que la clase Java es una entidad que se debe mapear a una tabla en la base de datos.

2. Especificar el nombre de la tabla en la base de datos: Puedes especificar el nombre de la tabla usando la anotación `@Table`. Si la clase Java se llama "Person", por ejemplo, y quieres que se mapee a una tabla llamada "personas" en la base de datos, deberías usar la siguiente anotación:

```java
@Entity
@Table(name = "personas")
public class Person {
    //...
}
```

3. Especificar el identificador de la entidad: Cada entidad en JPA debe tener un identificador único que se utiliza para acceder y manipular la entidad. El identificador se puede especificar usando la anotación `@Id`. Además, es posible que necesites especificar el tipo de identificador usando otras anotaciones, como `@GeneratedValue` si quieres que el identificador se genere automáticamente.

```java
@Entity
@Table(name = "personas")
public class Person {
    
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    //...
}
```

4. Definir los atributos de la entidad: Los atributos de la entidad corresponden a las columnas en la tabla de la base de datos. Puedes especificar el nombre de la columna utilizando la anotación `@Column`. 

```java
@Entity
@Table(name = "personas")
public class Person {
    
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
    
    @Column(name = "nombre")
    private String name;
    
    @Column(name = "edad")
    private int age;

    //...
}
```

En resumen, para definir una entidad en JPA Spring Data, debes anotar la clase Java con la anotación `@Entity`, especificar el nombre de la tabla en la base de datos utilizando la anotación `@Table`, definir el identificador de la entidad utilizando la anotación `@Id`, y definir los atributos de la entidad utilizando la anotación `@Column`.

## Relaciones entre entidades
En Spring Data JPA, una relación es una asociación entre dos o más entidades. Hay varios tipos de relaciones que se pueden establecer entre entidades, entre los cuales se encuentran:

1. Relación Uno a Uno (OneToOne): Esta relación se establece cuando una entidad se asocia con exactamente otra entidad. Por ejemplo, una entidad "Persona" podría tener una relación Uno a Uno con otra entidad "Dirección".

```java
@Entity
public class Person {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @OneToOne(mappedBy = "person")
    private Address address;

    // getters and setters
}

@Entity
public class Address {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String street;
    
    private String city;
    
    @OneToOne
    @JoinColumn(name = "person_id")
    private Person person;

    // getters and setters
}
```

2. Relación Uno a Muchos (OneToMany): Esta relación se establece cuando una entidad se asocia con varias instancias de otra entidad. Por ejemplo, una entidad "Equipo" podría tener una relación Uno a Muchos con una entidad "Jugador".

```java
@Entity
public class Team {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @OneToMany(mappedBy = "team")
    private List<Player> players;

    // getters and setters
}

@Entity
public class Player {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    private int number;
    
    @ManyToOne
    @JoinColumn(name = "team_id")
    private Team team;

    // getters and setters
}
```

3. Relación Muchos a Muchos (ManyToMany): Esta relación se establece cuando una entidad se asocia con varias instancias de otra entidad y viceversa. Por ejemplo, una entidad "Estudiante" podría tener una relación Muchos a Muchos con una entidad "Curso".

```java
@Entity
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @ManyToMany(mappedBy = "students")
    private List<Course> courses;

    // getters and setters
}

@Entity
public class Course {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @ManyToMany
    @JoinTable(
        name = "course_student",
        joinColumns = @JoinColumn(name = "course_id"),
        inverseJoinColumns = @JoinColumn(name = "student_id")
    )
    private List<Student> students;

    // getters and setters
}
```

En resumen, en Spring Data JPA, las relaciones se establecen mediante anotaciones como `@OneToOne`, `@OneToMany` y `@ManyToMany`. Estas anotaciones indican cómo se asocian las entidades entre sí y pueden proporcionar información sobre la clave externa, la tabla de unión, entre otros detalles necesarios para definir correctamente la relación.



## Excepciones Personalizadas
En Spring Data JPA, las excepciones personalizadas se utilizan para manejar errores específicos de la aplicación. Por ejemplo, si intentas guardar una entidad con un nombre que ya existe en la base de datos, se lanzará una excepción de tipo `DataIntegrityViolationException`. Sin embargo, en lugar de lanzar esta excepción, puedes crear tu propia excepción personalizada y lanzarla en su lugar. O por ejemplo, si vas a buscar una entidad por su id y no existe, se lanzará una excepción de tipo `EmptyResultDataAccessException`. En lugar de lanzar esta excepción, puedes crear tu propia excepción personalizada y lanzarla en su lugar.

De esta manera podemos manejar los errores de una manera más específica y personalizada. Para crear una excepción personalizada, debes crear una clase que extienda de `RuntimeException` y agregar un constructor que reciba un mensaje y una causa.

Además podemos añadir la anotación `@ResponseStatus` para indicar el estado que queremos devolver cuando salte la excepción. Por ejemplo, si queremos devolver un estado 404, podemos agregar la anotación `@ResponseStatus(HttpStatus.NOT_FOUND)`. Por ejemplo:

```java
// Nos permite devolver un estado cuando salta la excepción
@ResponseStatus(HttpStatus.NOT_FOUND)
public class RaquetaNotFoundException extends RaquetaException {
    // Por si debemos serializar
    @Serial
    private static final long serialVersionUID = 43876691117560211L;

    public RaquetaNotFoundException(String mensaje) {
        super(mensaje);
    }
}
```

## Cargando Datos de Prueba
Se pueden cargar datos de prueba o definir cualquier script sql para ejecutarlo antes de que se inicie la aplicación. Para ello, debemos crear un archivo llamado `data.sql` en la carpeta `resources` y escribir el script sql que queremos ejecutar. Por ejemplo:

```sql
INSERT INTO user (id, firstname, lastname, phone, mail) values (1, 'Roger', 'Federer', 623456781, 'roger@goat.com');
INSERT INTO user (id, firstname, lastname, phone, mail) values (2, 'Juan', 'Ferrer', 623456782, 'juan@goat.com');
INSERT INTO user (id, firstname, lastname, phone, mail) values (3, 'Antonio', 'Atleti', 623456783, 'antonio@goat.com');
INSERT INTO user (id, firstname, lastname, phone, mail) values (4, 'Rafael', 'Nadal', 623456784, 'rafa@goat.com');
INSERT INTO user (id, firstname, lastname, phone, mail) values (5, 'Stan', 'Wawrinka', 623456785, 'stan@goat.com');
```

## H2

H2 es una base de datos relacional que se encuentra escrita en Java y funciona como una base de datos en memoria, cuyo uno de sus puntos fuertes es que puede trabajar como una base de datos embebida en aplicaciones Java.

Esto quiere decir que no reemplaza de ninguna manera a MySQL, SQL Server u Oracle que deberán seguir siendo usados en ambientes productivos.

El propósito de H2 es agilizar el proceso de desarrollo. H2 puede crear la estructura de las tablas basándose en nuestras entitys JPA Cargar datos iniciales para nuestras pruebas, nos permitirá hacer transacciones CRUD como cualquier otra base datos con la diferencia que serán temporales, es decir los datos regresaran al estado original respetando el script inicial de carga. El uso de H2 es muy habitual en la parte de testing de desarrollos.


Para configurar H2 en un proyecto, debemos agregar la siguiente dependencia en el archivo pom.xml:

```xml
<dependency>
	<groupId>com.h2database</groupId>
	<artifactId>h2</artifactId>
	<scope>runtime</scope>
</dependency>
```

Además agregar la siguiente configuración en el fichero .properties:

```properties
spring.datasource.url=jdbc:h2:mem:testdb;NON_KEYWORDS=user,order
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect

spring.h2.console.path=/h2-console
spring.h2.console.enabled=true
spring.jpa.defer-datasource-initialization=true
```

OJO: Se añade en NON_KEYWORDS, las entidades definidas para que no se confundan en la creación de la sintaxis sql.


Una vez arranca la aplicación podemos acceder a la consola a través de este endpoint 

> http://localhost:8080/h2-console.

## MySql
Para conectarnos a una base de datos Mysql, añadir la dependencia en el POM y añadir las propiedades en application.properties .

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/nombre_bbdd
spring.datasource.username=root
spring.datasource.password=
spring.jpa.show-sql=true
spring.jpa.generate-ddl=true
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
```

```xml
   <!-- MySQL JDBC Driver -->
    <dependency>
        <groupId>com.mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
    </dependency>
```



