summary: Integración operaciones CRUD en MongoDB - API REST
id: BACK02
categories: api
tags: back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# BACK02 - Integración operaciones CRUD en MongoDB - API REST
<!-- ------------------------ -->
## Overview 
Duration: 100

### BACK02 - Integración operaciones CRUD en MongoDB - API REST - 100 min

**BACK02 - Integración operaciones CRUD en MongoDB - API REST**

En este codelab integraremos Mongo DB para realizar las operaciones CRUD asociadas a las peticiones REST implementadas en los codelabs anteriores. Aprenderemos a crear un microservicio spring boot que utilizará **Spring Data MongoDB** (ver artefacto **spring-boot-starter-data-mongodb** en el pom.xml del paso 1) para crear una aplicación que almacenará y recuperará datos de MongoDB, una de las bases de datos **NoSQL** orientada a *documentos* más populares*.* 

Requisitos previos: Tener instalado Mongo DB. 

Tenemos que añadir los **parámetros de conexión a Mongo DB**. Para ello insertar las lineas de abajo en el archivo de **properties** del proyecto. Está ubicado en: src/main/resources/*application.properties*:

```java
server.port = 8081
spring.data.mongodb.host=localhost
spring.data.mongodb.port=27017
#spring.data.mongodb.username=admin
#spring.data.mongodb.password=admin
spring.data.mongodb.database=productos_db
```

Para simplificar el ejercicio se han comentado los parámetros de autenticación (precedidos por #) así que podremos conectarnos sin credenciales (solo para el ejemplo, no recomendado).

1/ La configuración del fichero **pom.xml** del proyecto es la siguiente:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.3.4.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.techu.apirest</groupId>
	<artifactId>demo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>demo</name>
	<description>Demo project for Spring Boot</description>

	<properties>
		<java.version>14</java.version>
	</properties>
	
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-mongodb</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
			<exclusions>
				<exclusion>
					<groupId>org.junit.vintage</groupId>
					<artifactId>junit-vintage-engine</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>org.jetbrains</groupId>
			<artifactId>annotations</artifactId>
			<version>RELEASE</version>
			<scope>compile</scope>
		</dependency>
	</dependencies>
	
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
```

2/ Ahora crearemos un POJO  muy sencillo para representar un **ProductoModel** que será el que almacenemos en *MongoDB*  y que asociaremos con el recurso **productos**.

```java
package com.techu.apirest.model;

import org.jetbrains.annotations.NotNull;
import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;

@Document(collection="productos")
public class ProductoModel {

    @Id
    private String id;
    @NotNull
    private String descripcion;
    private Double precio;
    
    public ProductoModel() {}
    
    public ProductoModel(String id, String descripcion, Double precio) {
        this.id = id;
        this.descripcion = descripcion;
        this.precio = precio;
    }
    
    public String getId() {
        return id;
    }
    
    public void setId(String id) {
        this.id = id;
    }
    
    public String getDescripcion() {
        return descripcion;
    }
    
    public void setDescripcion(String descripcion) {
        this.descripcion = descripcion;
    }
    
    public Double getPrecio() {
        return precio;
    }
    
    public void setPrecio(Double precio) {
        this.precio = precio;
    }
}
```

3/ Ahora crearemos el **interface MongoRepository** del repositorio que permita realizar las operaciones CRUD sobre el objeto de la clase**ProductoModel**.

```java
package com.techu.apirest.repository;

import com.techu.apirest.model.ProductoModel;
import org.springframework.data.mongodb.repository.MongoRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface ProductoRepository extends MongoRepository<ProductoModel, String> {
}
```

Este repositorio es una interfaz que permite realizar varias operaciones que involucran a los objetos de la clase `Producto`. Obtiene estas operaciones extendiendo `ProductoRepository` de `MongoRepository`.

https://docs.spring.io/spring-data/mongodb/docs/current/api/org/springframework/data/mongodb/repository/MongoRepository.html

4/ Lo siguiente que haremos será implementar el servicio **ProductoService** que se conecte al repositorio de productos del paso anterior:

```java
package com.techu.apirest.service;

import com.techu.apirest.model.ProductoModel;
import com.techu.apirest.repository.ProductoRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Optional;

@Service
public class ProductoService {

    @Autowired
    ProductoRepository productoRepository;
    
    public List<ProductoModel> findAll() {
        return productoRepository.findAll();
    }
    
    public Optional<ProductoModel> findById(String  id) {
        return productoRepository.findById(id);
    }
    
    public ProductoModel save(ProductoModel entity) {
        return productoRepository.save(entity);
    }
    
    public boolean delete(ProductoModel entity) {
        try {
            productoRepository.delete(entity);
            return true;
        } catch(Exception ex) {
            return false;
        }
    }

}
```

5/ A continuación la implementación del **ProductoController**:

```java
package com.techu.apirest.controller;

import com.techu.apirest.model.ProductoModel;
import com.techu.apirest.service.ProductoService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.Optional;

@RestController
@RequestMapping("/apitechu/v2")
public class ProductoController {

    @Autowired
    ProductoService productoService;
    
    @GetMapping("/productos")
    public List<ProductoModel> getProductos() {
        return productoService.findAll();
    }
    
    @GetMapping("/productos/{id}" )
    public Optional<ProductoModel> getProductoId(@PathVariable String id){
        return productoService.findById(id);
    }
    
    @PostMapping("/productos")
    public ProductoModel postProductos(@RequestBody ProductoModel newProducto){
        productoService.save(newProducto);
        return newProducto;
    }
    
    @PutMapping("/productos")
    public void putProductos(@RequestBody ProductoModel productoToUpdate){
        productoService.save(productoToUpdate);
    }
    
    @DeleteMapping("/productos")
    public boolean deleteProductos(@RequestBody ProductoModel productoToDelete){
        return productoService.delete(productoToDelete);
    }

}
```

6/ Por último ejecutar la aplicación y probar en postman las operaciones CRUD implementadas. Mediante un cliente MongoDB (Robo 3T por ejemplo) confirmar las pruebas en la base de datos productos_db:

server.port = 8081
spring.data.mongodb.host=localhost
spring.data.mongodb.port=27017

Referencias:

http://www.robertocrespo.net/kaizen/implementar-microservicios-spring-boot-iii-acceso-datos-mongodb-data/

https://spring.io/guides/gs/accessing-mongodb-data-rest/#scratch

https://docs.spring.io/spring-data/mongodb/docs/current/api/org/springframework/data/mongodb/repository/MongoRepository.html

https://www.adictosaltrabajo.com/2015/03/02/optional-java-8/