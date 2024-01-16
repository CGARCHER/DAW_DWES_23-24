# Otros
- [Profiles](#profiles)

## Profiles

Dado que las aplicaciones se pueden ejecutar en diferentes entornos(Local, desarrollo, preproducción o producción). La información almacenada en los ficheros .properties puede
variar dependiendo de cada entorno, como por ejemplo: la configuración de acceso a una BBDD, cada entorno tiene su propia BBDD. La nomenclatura del fichero .properties será `application-{nombre_perfil}.properties`

Por ejemplo, en local voy a trabajar con una BBDD H2 y en el entorno de desarrollo se está usando una BBDD postgres.

`application-local.properties`:
```properties
spring.datasource.url=jdbc:h2:mem:testdb;NON_KEYWORDS=pokemon,coach
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect

spring.h2.console.path=/h2-console
spring.h2.console.enabled=true
spring.jpa.defer-datasource-initialization=true
```

`application-dev.properties`:
```properties
spring.datasource.url=jdbc:postgresql://10.85.12.1:5432/postgres
spring.datasource.username=root
spring.datasource.password=root
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto = none
spring.jpa.defer-datasource-initialization=true
```

Luego en el fichero `application.properties` se define que prefil está activo, además dispondrá de valores comunes que sean los mismos para todos los entornos.
```properties
spring.profiles.active=local
```


