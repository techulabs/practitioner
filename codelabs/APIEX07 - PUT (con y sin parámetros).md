**APIEX07 - PUT (con y sin parámetros)** 

Probar el siguiente bloque de código para hacer una petición PUT sin parámetros en la URL. Para añadir parámetros ver el codelab anterior APIEX04 - GET (con y sin parámetros).

```java
@PutMapping("/productos/{id}")
public Producto updateProducto(@PathVariable Integer id,
                           @RequestBody Producto producto) {
          return dataService.updateProducto(id-1, producto);
}
```

