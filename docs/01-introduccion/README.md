## **¿Qué es Spring?** ##

Spring framework es un conjunto de proyectos de código abierto desarrollados en
Java, con el objetivo de agilizar el desarrollo de aplicaciones en este lenguaje.
Todo su pack de aplicaciones es conocido como Spring platform e incluye
herramientas para el desarrollo web, microservicios, manejo de base de datos,
seguridad, entre otros.

### ¿Qué puede hacer Spring? ###

Cada una de estas características de Spring fueron desarrolladas en una serie de
proyectos independientes, donde cada uno es utilizado para diferentes fines. Entre los
principales proyectos se encuentran:

- Spring Boot: Facilita la creación y configuración inicial de proyectos de
Spring para generar aplicaciones de fácil y rápida puesta en marcha.

- Spring Data: Utilizado para la administración, manejo y comunicación con
bases de datos, tanto relacionales como no-relacionales.

- Spring Security: Utilizado para las cuestiones de seguridad que puede
necesitar todo proyecto.

- Spring Web Services: Utilizado para facilitar el desarrollo de Web Services
SOAP.

[Lista completa de todos los proyectos de Spring](https://spring.io/projects)

### Inversión de Control
Inversión de control (Inversion of Control en inglés, IoC) es un principio de diseño de software en el que el flujo de ejecución de un programa se invierte respecto a los métodos de programación tradicionales. En su lugar, en la inversión de control se especifican respuestas deseadas a sucesos o solicitudes de datos concretas, dejando que algún tipo de entidad o arquitectura externa lleve a cabo las acciones de control que se requieran en el orden necesario y para el conjunto de sucesos que tengan que ocurrir.

### Inyección de Dependencias
 La inyección de dependencias (en inglés Dependency Injection, DI) es un patrón de diseño orientado a objetos, en el que se suministran objetos a una clase en lugar de ser la propia clase la que cree dichos objetos. Esos objetos cumplen contratos que necesitan nuestras clases para poder funcionar (de ahí el concepto de dependencia). Nuestras clases no crean los objetos que necesitan, sino que se los suministra otra clase 'contenedora' que inyectará la implementación deseada a nuestro contrato.

## **¿Qué es Spring boot ?**


El objetivo principal de Spring Boot es el de facilitar al desarrollador la labor de acometer un proyecto web, creando toda la arquitectura, así como estableciendo las configuraciones iniciales necesarias.
La tecnología Spring Boot cuenta con un tomcat embebido, por lo que no es necesario desplegarla en ningún servidor, facilitando de esta forma su puesta en marcha.
Es importante no olvidarse que Spring Boot no es una tecnología aislada dentro del conjunto de Spring, la gran mayoría de restantes tecnologías de Spring; Spring framework, Spring data, Spring security, etc… tienen cabida aquí.

Spring Boot es una extensión de Spring Framework y se encuentra dentro de su
lista de proyectos. Fue creado con la finalidad de facilitar la creación de
aplicaciones web listas para salir a producción, es decir, bajo el concepto “Just
Run” (solo ejecutar).

Anteriormente, realizar las configuraciones iniciales para llevar a cabo una
aplicación en Spring llevaba mucho tiempo a los desarrolladores. Esto, se
realizaba mediante una configuración manual de un archivo xml y de un servidor
de aplicaciones web, consumiendo gran parte del tiempo de desarrollo del
proyecto en realizar configuraciones. Dada esta problemática y con la finalidad de
resolverla, fue desarrollado Spring Boot, que requiere una configuración mínima y
que puede ser integrado con otros proyectos de Spring o librerías externas.

### **Starters** ###

Spring Boot nos proporciona una serie de dependencias, llamadas starters, que podemos añadir a nuestro proyecto dependiendo de lo que necesitemos: crear un controlador REST, acceder a una base de datos usando JDBC, conectar con una cola de mensajes Apache ActiveMQ, etc.

Una vez añadimos un starter, éste nos proporciona todas las dependencias que necesitamos, tanto de Spring como de terceros. Además, los starters vienen configurados con valores por defecto, que pretenden minimizar la necesidad de configuración a la hora de desarrollar. Un buen ejemplo sería _**spring-boot-starter-web**_

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <version>3.1.5</version>
</dependency>
```


[POM](https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-web/3.1.5/spring-boot-starter-web-3.1.5.pom) de spring-boot-starter-web.
Se puede ver en su definición incluye en la sección Dependencies Las cuales ya están incluidas en el propio started y no hay que definirlas para poder usarlas en nuestro proyecto.


## **¿Qué es Maven?** ##

Maven es una herramienta de software para la gestión y construcción de proyectos
Java que se caracteriza por tener un modelo de configuración muy simple, basado
en el formato XML.

Maven utiliza el conocido archivo POM.xml (Proyect Object Model) para dentro de
él especificar las diferentes dependencias o librerías que serán necesarias incluir
en el proyecto que se esté desarrollando. A partir de esta definición, Maven se
encarga de buscar todas las dependencias especificadas en las versiones
correspondientes y que sean compatibles entre sí para el desarrollo de la
aplicación en cuestión, ahorrando gran cantidad de tiempo en lo que respecta a la
organización de los complementos necesarios.
