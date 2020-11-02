summary: Seguridad: Autenticación API REST basada en tokens con Spring y JWT
id: APIEX11
categories: API
tags: back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# APIEX11 - Seguridad: Autenticación API REST basada en tokens con Spring y JWT
<!-- ------------------------ -->
## Overview 
Duration: 60

**APIEX11 - Seguridad: Autenticación API REST basada en tokens con Spring y JWT**

En este codelab veremos una manera sencilla de **autenticar** las peticiones a una API mediante **tokens**, para poder garantizar que los usuarios que consumen nuestros servicios tienen permisos para hacerlo y son quien dicen ser. 

1. Crear un API REST con Spring Boot para proteger los recursos publicados en el API en el siguiente paso. El *pom.xml* deberá incluir las mismas dependencias que el siguiente:

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
   	<groupId>com.techu.security</groupId>
   	<artifactId>jwt</artifactId>
   	<version>0.0.1-SNAPSHOT</version>
   	<name>jwt</name>
   	<description>Spring Boot for create/handling jwt</description>
   
   <properties>
   	<java.version>15</java.version>
   </properties>
   
   <dependencies>
   	<dependency>
   		<groupId>org.springframework.boot</groupId>
   		<artifactId>spring-boot-starter-web</artifactId>
   	</dependency>
   	<dependency>
   		<groupId>org.bitbucket.b_c</groupId>
   		<artifactId>jose4j</artifactId>
   		<version>0.6.5</version>
   	</dependency>
   	<dependency>
   		<groupId>com.kastkode</groupId>
   		<artifactId>springsandwich</artifactId>
   		<version>[1.0.2,)</version>
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

2. Ahora vamos a definir un controlador REST para responder a todas las peticiones del endpoint **/apitechu/jwt/hello**, que simplemente devuelva un mensaje de bienvenida a todos los clientes que tengan autorización para acceder al servicio.

   El controlador *HelloWorldController* contendrá la lógica para obtener un **token de acceso.**

```java
package com.techu.security.jwt.controller;

import com.kastkode.springsandwich.filter.annotation.Before;
import com.kastkode.springsandwich.filter.annotation.BeforeElement;
import com.techu.security.jwt.components.AuthHandler;
import com.techu.security.jwt.components.JWTBuilder;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/apitechu/jwt")
public class HelloWorldController {
    @Autowired
    JWTBuilder jwtBuilder;

	@GetMapping("/tokenget")
	public String tokenget(@RequestParam(value="nombre", defaultValue="Tech U!") String name){
        return jwtBuilder.generateToken(name,"admin");
    }
    
    @GetMapping(path="/hello",headers = {"Authorization"})
    @Before(@BeforeElement(AuthHandler.class))
    public String helloWorld(){
        String s = "Hello from JWT demo...";
        return s;
    }
}
```

Como podemos observar, previamente debemos hacer una petición al endpoint **/tokenget** para que nos devuelva un token JWT a partir del parámetro 'nombre' que recibe. Y despúes se llama al método **generateToken()** con el string "admin" y el parámetro 'name' para generar el token desde la clase que debemos añadir en el siguiente paso.

3/ La clase **JWTBuilder**.

```java
package com.techu.security.jwt.components;

import org.jose4j.jwa.AlgorithmConstraints;
import org.jose4j.jwk.RsaJsonWebKey;
import org.jose4j.jwk.RsaJwkGenerator;
import org.jose4j.jws.AlgorithmIdentifiers;
import org.jose4j.jws.JsonWebSignature;
import org.jose4j.jwt.JwtClaims;
import org.jose4j.jwt.MalformedClaimException;
import org.jose4j.jwt.consumer.ErrorCodes;
import org.jose4j.jwt.consumer.InvalidJwtException;
import org.jose4j.jwt.consumer.JwtConsumer;
import org.jose4j.jwt.consumer.JwtConsumerBuilder;
import org.jose4j.lang.JoseException;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

import javax.annotation.PostConstruct;
import javax.security.auth.message.AuthException;

@Component
public class JWTBuilder {
    @Value("${jwt.issuer}")
    private String jwtIssuer;
    @Value("${jwt.secret}")
    private String jwtSecret;
    @Value("${jwt.expiry}")
    private Float jwtExpiry;
    RsaJsonWebKey rsaJsonWebKey;

    public JWTBuilder(){}
    
    public String generateToken (String usersID,String roles){
        try{
            JwtClaims jwtClaims = new JwtClaims();
            jwtClaims.setIssuer(jwtIssuer);
            jwtClaims.setExpirationTimeMinutesInTheFuture(jwtExpiry);
    
            jwtClaims.setAudience("ALL");
            jwtClaims.setStringListClaim("groups",roles);
            jwtClaims.setGeneratedJwtId();
            jwtClaims.setIssuedAtToNow();
            jwtClaims.setSubject("AUTHTOKEN");
            jwtClaims.setClaim("userID", usersID);
            JsonWebSignature jws = new JsonWebSignature();
            jws.setPayload(jwtClaims.toJson());
            jws.setKey(rsaJsonWebKey.getPrivateKey());
            jws.setKeyIdHeaderValue(rsaJsonWebKey.getKeyId());
            jws.setAlgorithmHeaderValue(AlgorithmIdentifiers.RSA_USING_SHA256);
    
            return jws.getCompactSerialization();
        } catch (JoseException e){
            e.printStackTrace();
            return null;
        }
    }
    
    public JwtClaims generateParseToken(String token) throws Exception {
        JwtConsumer jwtConsumer = new JwtConsumerBuilder()
                .setRequireExpirationTime()
                .setSkipSignatureVerification()
                .setAllowedClockSkewInSeconds(60)
                .setRequireSubject()
                .setExpectedIssuer(jwtIssuer)
                .setExpectedAudience("ALL")
                .setExpectedSubject("AUTHTOKEN")
                .setVerificationKey(rsaJsonWebKey.getKey())
                .setJwsAlgorithmConstraints(new AlgorithmConstraints(AlgorithmConstraints.ConstraintType.WHITELIST, AlgorithmIdentifiers.RSA_USING_SHA256))
                .build();
        try {
            JwtClaims jwtClaims = jwtConsumer.processToClaims(token);
            return jwtClaims;
        } catch (InvalidJwtException e) {
            try {
                if (e.hasExpired()) {
                    throw new Exception("JWT expired at " + e.getJwtContext().getJwtClaims().getExpirationTime());
                }
                if (e.hasErrorCode(ErrorCodes.AUDIENCE_INVALID)) {
                    throw new AuthException("JWT had wrong audience: " + e.getJwtContext().getJwtClaims().getAudience());
                }
                throw new AuthException(e.getMessage());
            } catch (MalformedClaimException innerE) {
                throw new AuthException("invalid Token");
            }
        }
    }
    
    @PostConstruct
    public void init(){
        try{
            rsaJsonWebKey = RsaJwkGenerator.generateJwk(2048);
            rsaJsonWebKey.setKeyId(jwtSecret);
        } catch (JoseException e){
            e.printStackTrace();
        }
    }

}
```

Utilizamos el método *generateToken()* para construir el token que, como veremos en el siguiente paso, usaremos para autorizar las peticiones a los recursos protegidos. 

4/ Para hacer el mapeo de las propiedades *jwtIssuer*, *jwtExpiry* y *jwtSecret* anotadas con **@Value**, tenemos que añadir las siguientes propiedades al archivo de properties **application.properties**:

`server.port = 8080`
`jwt.issuer = techu`
`jwt.secret = shhhhh`
`jwt.expiry = 10`

5/ Ahora, añadiremos la clase *AuthHandler* que validará si el token es correcto.

```java
package com.techu.security.jwt.components;

import com.kastkode.springsandwich.filter.api.BeforeHandler;
import com.kastkode.springsandwich.filter.api.Flow;
import org.jetbrains.annotations.NotNull;
import org.jose4j.jwt.JwtClaims;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import org.springframework.web.method.HandlerMethod;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
public class AuthHandler implements BeforeHandler {
    Logger logger = LoggerFactory.getLogger(AuthHandler.class);


@Autowired
JWTBuilder jwtBuilder;

@Override
public Flow handle(@NotNull HttpServletRequest request, HttpServletResponse response, HandlerMethod handler, String[] flags) throws Exception{
    logger.debug(request.getMethod() + " request is executing on " + request.getRequestURI());
    String token = request.getHeader("Authorization");

    if (token == null) {
        throw new RuntimeException("Auth token is required");
    }

    JwtClaims claims = jwtBuilder.generateParseToken(token);
    request.setAttribute("userId",claims.getClaimValue("userID").toString());
    //check if the user is still valid
    return Flow.CONTINUE;
}
}
```

6/ Por último hacer la petición al endpoint */hello* pasándole el token JWT en la cabecera HTTP **Authorization**. Y comprueba que tienes autorización y recibes el mensaje `"Hello from JWT demo..."`.