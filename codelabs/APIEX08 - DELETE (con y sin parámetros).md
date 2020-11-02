summary: DELETE (con y sin parámetros)
id: APIEX08
categories: API
tags: back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# APIEX08 - DELETE (con y sin parámetros)
<!-- ------------------------ -->
## Overview 
Duration: 25

**APIEX08 - DELETE (con y sin parámetros)** 

Probar el siguiente bloque de código para hacer la petición DELETE sin parámetros en la URL. Para añadir parámetros ver el codelab anterior APIEX04 - GET (con y sin parámetros).

```java
@DeleteMapping("/productos/{id}")
 public void deleteProducto(@PathVariable Integer id){
       dataService.removeProducto(id-1);
 }
```

