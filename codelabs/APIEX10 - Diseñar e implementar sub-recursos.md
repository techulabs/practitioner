summary: Diseñar e implementar sub-recursos
id: APIEX10
categories: API
tags: back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# APIEX10 - Diseñar e implementar sub-recursos
<!-- ------------------------ -->
## Overview 
Duration: 25

**APIEX10 - Diseñar e implementar sub-recursos**

Siguiendo las buenas prácticas de diseño de una API REST y de cara a representar las posibles relaciones entre los recursos/entidades podemos definir el endpoint como sigue:

`GET /recurso/{id}/subrecurso`

Este endpoint representa una relación uno (recurso) a muchos (subrecurso).

1. Supongamos que en el modelo de datos de nuestro proyecto existe una relación entre *Producto* y *User* (uno a muchos) que represente los usuarios que consumen un determinado producto. Antes que nada, tenemos que añadir al modelo la clase *UserModel* (que será el subrecurso):

   ```java
   package com.apirest.models;
   
   public class UserModel {
       
       private String userId;
       
       public UserModel(){}
       
       public UserModel(String userId){
           this.userId = userId;
       }
       
       public String getUserId(){
           return this.userId;
       }
   }
   ```

2. Ahora tenemos que añadir una propiedad (*users*) a la clase *ProductoModel* con la lista de usuarios del recurso productos, quedando la clase de la siguiente forma:

   ```java
   package com.apirest.models;
   import java.util.List;
   import java.util.Objects;
   
   public class ProductoModel {
       private long id;
       private String descripcion;
       private double precio;
       private List<UserModel> users;
   
       public ProductoModel() {
       }
   
       public ProductoModel(long id, String descripcion, double precio) {
           this.id = id;
           this.descripcion = descripcion;
           this.precio = precio;
       }
   
       public ProductoModel(long id, String descripcion, double precio, List<UserModel> users) {
           this.id = id;
           this.descripcion = descripcion;
           this.precio = precio;
           this.users = users;
       }
   
       public long getId() {
           return id;
       }
   
       public String getDescripcion() {
           return descripcion;
       }
   
       public double getPrecio() {
           return precio;
       }
   
       public void setDescripcion(String descripcion) {
           this.descripcion = descripcion;
       }
   
       public void setPrecio(double precio) {
           this.precio = precio;
       }
   
       public List<UserModel> getUsers(){
           return this.users;
       }
   
       public void setUsers(List<UserModel> users) {
           this.users = users;
       }
   
   ```

   

3. Y por último, nos toca implementamar en *ProductoController* el servicio que enviará los usuarios de un determinado producto mediante su ID:

   ```java
   @GetMapping("/productos/{id}/users")
   public List<UserModel> getUsers(@PathVariable int id{
    ProductoModel pr =                              productService.getProducto(id);
    return pr.getUsers();
   }
   ```

---

