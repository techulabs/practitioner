summary: PATCH
id: APIEX09
categories: API
tags: back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# APIEX09 - PATCH
<!-- ------------------------ -->
## Overview 
Duration: 25

**APIEX09 - PATCH**

Ya hemos visto el método PUT para realizar una actualización total (todos o casi todos los campos) de un recurso.  Pero, ¿qué pasa cuándo el cliente necesita actualizar un único campo del recurso? Al enviar todo el objeto que representa el recurso a actualizar estaríamos consumiendo más ancho de banda del necesario y manejando más campos de la cuenta. Para estos casos tiene más sentido usar PATCH.

1. Ahora, supongamos que queremos actualizar sólo el campo *precio* de la entidad Producto. Para ello podemos crear una clase para representar una actualización parcial  del campo *precio*:

   ```java
   public class ProductoPrecioOnly {
       
       private long id;
       private double precio;
   
       public ProductoPrecioOnly() {}
       
       public ProductoPrecioOnly(double precio) {
           this.precio = precio;
       }
       
       public double getPrecio() {
           return precio;
       }
       
       public void setPrecio(double precio) {
           this.precio = precio;
       }
   }
   ```

   

2. A continuación, creamos el método *patchProducto* para enviar una actualización parcial de la siguiente forma:

   ```java
   @PatchMapping("/productos/{id}")
   public Producto patchProducto(@PathVariable long id, @RequestBody ProductoPrecioOnly proPrecio) {
       return dataService.updateProducto(id, proPrecio);
   }
   ```

   Con esta clase (DTO) más granular, estaríamos enviando solamente el campo que necesitamos actualizar sin la sobrecarga de enviar el objeto Producto completo.

3. Y por último, en el caso de que necesitemos hacer lo anterior para muchos campos (actualización parcial) entonces podríamos usar una estructura de datos como Map de Java para implementar una solución genérica:

   ```java
   @PatchMapping("/productos/{id}")
   public ResponseEntity<?> patchGeneric(@RequestBody Map<String, Object> updates, @PathVariable long id) {
       return dataService.save(id, updates);
   }
   ```

   Con esta solución podríamos actualizar el campo *descripcion* o el que queramos como sigue:

   ```java
   Map<String, Object> updates = new HashMap<>();
   updates.put("descripcion", "Calle Gran Vía")
       ...
   ```

   

REFERENCIAS:

https://www.baeldung.com/http-put-patch-difference-spring