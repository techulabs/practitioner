summary: Servicio REST con Spring y anotación @CrossOrigin
id: APIEX12
categories: API
tags: back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# APIEX12 - Servicio REST con Spring y anotación @CrossOrigin
<!-- ------------------------ -->
## Overview 
Duration: 60

**APIEX12 - Servicio REST con Spring y anotación @CrossOrigin**

Aquí vas a aprender paso a paso cómo solucionar el problema de Cross Origin Resource Sharing (CORS) que nos impide el acceso a un servicio REST.

1. Una vez instaladas las dependencias de web (spring-boot-starter-web) el siguiente paso es construirnos un Controlador (HolaMundoController) de tipo REST en el endpoint http://localhost:8080/saludo

```java
package com.techu.holamundo;
import org.springframework.web.bind.annotation.RequestMapping; import org.springframework.web.bind.annotation.RestController; 

@RestController 
public class HolaMundoController {  
 	@GetMapping("/saludo")
	public String saludar() {
    	return "Hola Tech U!";
 	}
}
```

2. Arrancar la aplicación hola mundo Spring Boot y solicitar la URL en postman o cualquier navegador. Confirm que al servicio REST nos responde sin ningún problema . 
3. Ahora bien esto se debe a que realizamos una invocación directa y no vía JavaScript. Si intentamos solicitar la misma URL con JavaScript a través de una petición AJAX nos encontraremos con un problema de Cross Origin Resource Sharing (CORS) que nos impide el acceso. Carga en el navegador el siguiente script que hace una petición directa a nuestro servicio web:

```html
<html>
<head>
<script type="text/javascript" src="jquery-3.3.1.min.js">
</script>
<script type="text/javascript">
$(document).ready(function() {
    $.get("http://localhost:8080/saludo",function(datos) {
     console.log(datos);
}) })
</script>
</head>
<body>
</html>
```

4. Observa en el inspector del navegador el mensaje de error que lanza:

`Access to XMLHttpRequest at 'http://localhost:8080/saludo' from 1.html:1 origin 'null' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.`

El recurso esta por defecto bloqueado para peticiones que no se hagan desde localhost, como es el caso de nuestra petición que se realiza desde un fichero directamente. 

5. Para solventar este problema es suficiente con modificar el servicio de Spring y añadir la anotación **@CrossOrigin** para que nos permite el acceso desde otras ubicaciones. Sólo por simplificar, para este ejemplo se va a permitir acceder al recurso desde cualquier lugar.

```java
package com.techu.holamundo;
import org.springframework.web.bind.annotation.RequestMapping; import org.springframework.web.bind.annotation.RestController; 

@RestController 
@CrossOrigin(origins = "*", methods= {RequestMethod.GET,RequestMethod.POST})
public class HolaMundoController {  

 	@GetMapping("/saludo")
	public String saludar() {
    	return "Hola Tech U!";
 	}
}
```

6. Volvemos a cargar el servidor y ahora si podremos acceder al recurso. 

Siempre que tengamos recursos REST y queramos acceder a ellos debemos usar Spring REST CORS y abrir el acceso remoto sino por defecto los datos de nuestros servicios no estarán accesibles.