
## Valid

La anotación @Valid ayuda a que Spring Boot valide el request antes de ejecutar el método.
Bean Validation proporciona un conjunto de anotaciones que puedes usar para validar los atributos de una clase.

Recuerda añadir el starter de validación en tu proyecto,

```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-validation</artifactId>
		</dependency>
```

Algunas de las anotaciones más comunes son:

- @NotNull: indica que el atributo no puede ser nulo.
- @Size: indica el tamaño mínimo y/o máximo permitido para un atributo de tipo String o Collection.
- @Min y @Max: indican el valor mínimo y/o máximo permitido para un atributo numérico.
- @Email: indica que el atributo debe ser una dirección de correo electrónico válida.
- @Past y @Future: indican que un atributo de tipo Date o Calendar debe ser una fecha en el pasado o en el futuro, respectivamente.
- @Pattern: indica un patrón que debe cumplir un atributo de tipo String.
- @DecimalMin y @DecimalMax: indican el valor mínimo y/o máximo permitido para un atributo de tipo BigDecimal.
- @NotEmpty: indica que un atributo de tipo String, Collection o Map no puede ser vacío.

```java

    @NotEmpty(message = "El ISBN no puede estar vacío")
    private String isbn;
    
    @NotNull(message = "El tipo no puede ser nulo")
    private Types type;
    
    @NotNull(message = "El estado no puede ser nulo")
    private Status status;
    
    @Min(value = 1, message = "El precio debe ser un número positivo")
    private double price;
    
    @NotNull(message = "La fecha de publicación no puede estar vacía")
    private LocalDate published;
```
Para que se apliquen la validación hay que usar la anotación
```java @valid BookDTO bookDTO ```

Por último, faltaría añadir en el handler del proyecto, el control de la excepción que se lanza y la respuesta que se devolverá a la petición recibida:


```java

  @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErrorResponse> handleMethodArgumentNotValidException(MethodArgumentNotValidException ex) {

        Map<String, String> errors = new HashMap<>();
        for(FieldError fieldError: ex.getBindingResult().getFieldErrors()){
            errors.put(fieldError.getField(),fieldError.getDefaultMessage());
        }
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(
                new FieldErrorResponse(ex.getMessage()
                        ,"Errores de validación de datos de entrada"
                        ,HttpStatus.BAD_REQUEST.value()
                        ,errors));

    }
```

Para validar enumerados, hay diferentes formas de validar un enumerado. Al tener un enum como atributo, el framework 
se encarga de realizar la conversión de String a Enum(Partimos de una petición HTTP). Cuando se pasa un valor no declarado(Anime) en el Enum,
aparece el siguiente mensaje en consola:

```console
 Resolved [org.springframework.http.converter.HttpMessageNotReadableException: JSON parse error: Cannot deserialize value of type `com.example.booksapirest.enums.TypeEnum` from String "ANIME": not one of the values accepted for Enum class: [COOKING, SPORT, DRAMA, SCIFI, MANGA]]
```

Una solución rápida y sencilla, sería captura la excepción HttpMessageNotReadableException en el handler de excepciones. Esta excepcion salta cuando un valor que pasa en la petición no se puede castear al tipo de dato definido esperado,
hay que llevar cuidado de no ocultar las excepciones, porque puede saltar otro fallo y no tiene porque ser de un enumerado.

```java



    @ExceptionHandler(HttpMessageNotReadableException.class)
    public ResponseEntity<ErrorResponse> handlerHttpMessageNotReadableException(HttpMessageNotReadableException ex) {
        String errorMessage = ex.getMessage();
        ErrorResponse errorResponse;

        if (errorMessage != null && errorMessage.contains("enum")) {
            errorResponse = new ErrorResponse(errorMessage, "Invalid enum value provided", HttpStatus.BAD_REQUEST.value());
        } else {
            errorResponse = new ErrorResponse(errorMessage, "Error", HttpStatus.BAD_REQUEST.value());
        }

        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(errorResponse);
    }


```
Si deseamos seguir con el flujo de validaciones, osea devolver un listado con los errores de validación producidos por los valores pasados en la request. Lo primero que hay que hacer 
es crear un Deserializer custom para que cuando no encuentre el valor establezca un valor null por defecto. Y luego hay que crear una validator (https://www.baeldung.com/javax-validations-enums#pattern-validation) para enum y usar la nueva anotación creada.

