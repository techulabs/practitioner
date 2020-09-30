summary: Testing JUnit
id: DVEX04
categories: DevOps
tags: back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# DVEX04 - Testing JUnit
<!-- ------------------------ -->
## Overview 
Duration: 15

**DVEX04 - Testing JUnit**

En este codelab vamos a realizar nuestro primer test unitario con JUnit. Para realizar este test, nos basaremos en el proyecto que creamos en el codelab  APIEX03: Implementar Hola Mundo API REST.

**1/** Antes que nada tenemos que añadir la dependencia de JUnit en el pom.xml del proyecto.

```xml
<dependency>
		<groupId>junit</groupId>
		<artifactId>junit</artifactId>
		<version>4.13</version>
		<scope>test</scope>
</dependency>
```

**2/** Lo siguiente que vamos a hacer es asegurarnos de tener la estructura de packages de tal manera que, ambas estructuras sean iguales: 

`src/main/java` 

`src/main/test` 

En la carpeta /test vamos a crear un **test case** para probar la clase HMController (HolaMundoController). 

La clase de testing, la identificamos bajo el mismo nombre que la del package src/main/java pero, en este caso, acompañada de la coletilla Test al final: **HolaMundoControllerTest**:

```java
package com.techu.apirest;

import org.junit.jupiter.api.Test;
import static org.junit.Assert.assertEquals;

public class HolaMundoControllerTest {

 @Test
 public void testCaseHM() {
	HMController HMController = new HMController();
	assertEquals("Hola equipo Tech U!",    HMController.saluda());
    }
}
```

Mediante la anotación @Test le indicamos que dicho método es un Test. 

Si nos fijamos en el Controlador a testear (HMController), podemos ver que nos devuelve un String. Por tanto, lo que compararemos mediante **Asserts**, será un String.

**3/** Ejecutar todos los tests de la clase HolaMundoControllerTest, y comprobar los resultados en el IDE utilizado.

---

