summary: POST (con y sin parámetros)
id: APIEX06
categories: API
tags: back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# APIEX06 - POST (con y sin parámetros)
<!-- ------------------------ -->
## Overview 
Duration: 25

**APIEX06 - POST (con y sin parámetros)** 

Probar el siguiente bloque de código para hacer peticiones POST sin parámetros en la URL. Para añadir parámetros ver el codelab anterior APIEX04 - GET (con y sin parámetros).

```java
@PostMapping("/productos")
        public Producto addProducto(@RequestBody Producto producto) {
            return dataService.addProducto(producto);
        }
```

