summary: Implementar Hola Mundo API REST
id: APIEX03
categories: API
tags: back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# APIEX03 - Implementar Hola Mundo API REST
<!-- ------------------------ -->
## Overview 
Duration: 45

##### APIEX03 - Implementar Hola Mundo API REST

Construiremos el "Hola Mundo" de un servicio web con Spring Boot que acepte peticiones en http://localhost:8080/saludo.

Para facilitarnos las cosas añadimos la dependencia **Spring Web** la cual nos creará un proyecto web básico.

A continuación añadimos el código Java de la clase HolaMundoController la cual será el controlador REST que acepte las peticiones:

```java
package com.techu.holamundo;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HolaMundoController {

    @RequestMapping("/saludo")
    public String saludar() {
        return "Hola Tech U!";
    }

}
```

La anotación @RequestMapping se ha simplificado por:

`@RequestMapping(method = RequestMethod.GET, path = "/saludo")`

Explicación del código: The class is flagged as a `@RestController`, meaning it is ready for use by Spring MVC to handle web requests. `@RequestMapping` maps `/` to the `index()` method. When invoked from a browser or by using curl on the command line, the method returns pure text. That is because `@RestController` combines `@Controller` and `@ResponseBody`, two annotations that results in web requests returning data rather than a view.

Por último ejecutamos la aplicación y en el comprobamos la petición en el navegador.

REFERENCIAS:

https://spring.io/guides/gs/spring-boot/

https://github.com/spring-guides/gs-spring-boot/tree/master/initial/src/main/java/com/example/springboot

RETO: 

*The created Spring Boot application has one endpoint available at [/saludo](http://localhost:8080/saludo). However, if you open the root context of your application at http://localhost:8080/, you will get an error because there is no root resource defined. Let's add a static HTML home page with links to your endpoint:* https://www.jetbrains.com/help/idea/your-first-spring-application.html#add-home-page