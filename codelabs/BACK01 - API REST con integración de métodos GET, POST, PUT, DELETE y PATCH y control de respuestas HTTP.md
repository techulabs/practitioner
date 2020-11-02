summary: API REST con integración de métodos GET, POST, PUT, DELETE y PATCH y control de respuestas HTTP
id: BACK01
categories: api
tags: back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# BACK01 - API REST con integración de métodos GET, POST, PUT, DELETE y PATCH y control de respuestas HTTP
<!-- ------------------------ -->
## Overview 
Duration: 100

### BACK01 - API REST con integración de métodos GET, POST, PUT, DELETE y PATCH y control de respuestas HTTP - 100 min

**BACK01** - **API REST con integración de métodos GET, POST, PUT, DELETE y PATCH y control de respuestas HTTP.**

El siguiente codelab integra todas las operaciones HTTP implementadas anteriormente y veremos como manejar la respuesta HTTP de los servicios  mediante **ResponseEntity**.

En ciertas circunstancias se necesita enviar respuestas HTTP desde nuestra aplicación backend hacia la aplicación cliente. Spring posee muchas formas para manejar esto, en este caso usaremos *ResponseEntity* una clase de Spring para manejar el cuerpo y estatus de estas respuestas.

El código fuente completo lo puedes obtener desde el siguiente repositorio Bitbucket:

https://bitbucket.org/llabor/backend-apirest-productos/src/master/

1. Vamos a centrarnos en la clase *ProductoController* que es en donde vamos a incluir el manejo de las respuestas HTTP.  Primero necesitamos importar la clase *ResponseEntity*:

   `import org.springframework.http.ResponseEntity;`

2. Empezaremos con el servicio que ya tenemos hecho para enviar un producto por su ID, y lo modificaremos para que devuelva el código HTTP 404 si no existe ese producto. En caso contrario, enviaremos el producto solicitado. Para ello lo que haremos es sustituir el tipo que devuelve el método getProductoId por ResponseEntity:

   ```java
   @GetMapping("/productos/{id}")
   public ResponseEntity getProductoId(@PathVariable int id) {
   ProductoModel pr = productService.getProducto(id);
   if (pr == null) {
       return new ResponseEntity<>("Producto no encontrado.", HttpStatus.NOT_FOUND);
   }
       return ResponseEntity.ok(pr);
   }
   ```

3. Procederemos ahora a hacer lo suyo con el resto de servicios y empezaremos con el método POST por ejemplo:

   ```java
   @PostMapping("/productos")
   public ResponseEntity<String> addProducto(@RequestBody ProductoModel productoModel) {
    productService.addProducto(productoModel);
    return new ResponseEntity<>("Product created successfully!", HttpStatus.CREATED);
   }
   ```

   En este caso, enviaremos como respuesta la confirmación de la creación del recurso mediante el código HTTP 201 (HttpStatus.CREATED).

4. Para la operación PUT enviaremos un 200 OK (HttpStatus.OK) si el producto a actualizar existe y un 404 Not Found en caso contrario.

   ```java
   @PutMapping("/productos/{id}")
   public ResponseEntity updateProducto(@PathVariable int id,
           @RequestBody ProductoModel productToUpdate) {
   ProductoModel pr = productService.getProducto(id);
   if (pr == null) {
     return new ResponseEntity<>("Producto no encontrado.", HttpStatus.NOT_FOUND);
    }
   productService.updateProducto(id-1, productToUpdate);
   return new ResponseEntity<>("Producto actualizado correctamente.", HttpStatus.OK); // Buenas prácticas dicen que mejor enviar 204 No Content
   }
   ```

   

5. En el método DELETE enviaremos el estado 200 genérico para respuestas exitosas o si queremos afinar un poco más el código 204 No Content (HttpStatus.NO_CONTENT):

   ```java
   DeleteMapping("/productos/{id}")
   public ResponseEntity deleteProducto(@PathVariable Integer id) {
   ProductoModel pr = productService.getProducto(id);
   if(pr == null) {
     return new ResponseEntity<>("Producto no encontrado.", HttpStatus.NOT_FOUND);
    }
    productService.removeProducto(id - 1);
    return new ResponseEntity<>(HttpStatus.NO_CONTENT); // status code 204-No Content
    }
   ```

   

6. PATCH

   ```java
   @PatchMapping("/productos/{id}")
   public ResponseEntity patchPrecioProducto(
     @RequestBody ProductoPrecioOnly productoPrecioOnly,                                @PathVariable int id){
     ProductoModel pr = productService.getProducto(id);
     if (pr == null) {
       return new ResponseEntity<>("Producto no encontrado.", HttpStatus.NOT_FOUND);
      }
      pr.setPrecio(productoPrecioOnly.getPrecio());
      productService.updateProducto(id-1, pr);
      return new ResponseEntity<>(pr, HttpStatus.OK);
   }
   ```

   

7. GET subrecurso

   ```java
   @GetMapping("/productos/{id}/users")
   public ResponseEntity getProductIdUsers(@PathVariable int id){
   ProductoModel pr = productService.getProducto(id);
   if (pr == null) {
     return new ResponseEntity<>("Producto no encontrado.", HttpStatus.NOT_FOUND);
   }
   if (pr.getUsers()!=null)
     return ResponseEntity.ok(pr.getUsers());
   else
     return new ResponseEntity<>(HttpStatus.NO_CONTENT);
   }
   ```

REFERENCIAS:

https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/http/ResponseEntity.html

https://www.baeldung.com/spring-response-entity