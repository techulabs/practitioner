summary: Documentación de nuestra API (Swagger + Swagger UI Tool)
id: APIEX13
categories: API
tags: back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# APIEX13 - Documentación de nuestra API (Swagger + Swagger UI Tool)
<!-- ------------------------ -->
## Overview 
Duration: 60

**APIEX13 - Documentación de nuestra API (Swagger + Swagger UI Tool)**

Para documentar nuestra API con **Swagger. SpringFox** tenemos que actualizar nuestro proyecto de la siguiente forma:

1. Añadir dependencias Swagger  y SpringFox en el pom.xml:

   <dependency>

   ​	<groupId>io.springfox</groupId>

   ​	<artifactId>springfox-swagger2</artifactId>

   ​	<version>2.9.2</version>

   </dependency>

   <dependency>

   ​	<groupId>io.springfox</groupId>

   ​	<artifactId>springfox-swagger-ui</artifactId>

   ​	<version>2.9.2</version>

   </dependency>

2. Crear la clase *SwaggerConfig* con la configuración básica que generará de forma automática el fichero *swagger.json* basado en el paquete (*com.apirest* en nuestro caso) que contiene los servicios REST:

   ```java
   @Configuration
   @EnableSwagger2
   public class SwaggerConfig {
   
   @Bean
   public Docket api() {
    // Docket es una clase de SpringFox
    return new Docket(DocumentationType.SWAGGER_2)
   .select()   		   .apis(RequestHandlerSelectors.basePackage("com.apirest"))
   .paths(PathSelectors.any())
   .build();
   }
       
   }
   ```

   

3. Finalmente, la documentación generada estará disponible en:

   ​		`localhost:8080/swagger-ui.html`

REFERENCIAS:

https://www.baeldung.com/swagger-2-documentation-for-spring-rest-api