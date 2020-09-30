**APIEX09 - DELETE (con y sin parámetros)** 

Probar el siguiente bloque de código para hacer la petición DELETE sin parámetros en la URL. Para añadir parámetros ver el codelab anterior APIEX04 - GET (con y sin parámetros).

```java
@DeleteMapping("/productos/{id}")
 public void deleteProducto(@PathVariable Integer id){
       dataService.removeProducto(id-1);
 }
```

