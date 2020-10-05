summary: Práctica GET (con variable en la URI) en Postman
id: APIEX05
categories: API
tags: back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# APIEX05 - Práctica GET (con variable en la URI) en Postman
<!-- ------------------------ -->
## Overview 
Duration: 25

**APIEX05 - Práctica GET (con variable en la URI) en Postman**

##### APIEX05 - Práctica GET (con variable en la URI) en Postman

Postman permite crear peticiones sobre APIs de una forma muy sencilla y poder, de esta manera, probar las APIs. 

1. Te proponemos que pruebes primero algunas peticiones de los ejercicios anteriores en Postman para familiarizarnos con esta herramienta.

2. Probar el siguiente bloque de código que permite enviar una variable en la URI mediante la anotación @PathVariable. El código completo puedes descargarlo desde este repositiorio Bitbucket:

   

```java
//@RequestMapping(method=RequestMethod.GET,path="/productos/{id}")
@GetMapping("/productos/{id}")
   public Producto getProductoId(@PathVariable Integer id) {
          return dataService.getProducto(id-1);
   }
```

3. Comprobar diferencia entre `@RequestParam` y `@PathVariable`. 