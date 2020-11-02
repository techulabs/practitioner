summary: Crear nuestro primer proyecto Maven con IntelliJ IDEA
id: GPEX01
categories: Proyectos
tags: back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# GPEX01 - Crear nuestro primer proyecto Maven con IntelliJ IDEA
<!-- ------------------------ -->
## Overview 
Duration: 30
##### **GPEX01** - Crear nuestro primer proyecto Maven con IntelliJ IDEA

1. En la ventana principal del IDE ir al Menu **File -> New -> Project…**

2. Se abrirá el Asistente **New Project**.

3. Selecciona en el desplegable el Project SDK (JDK).

4. Selecciona **Maven** en el panel izquierdo y luego marca la opción **Create from archetype**.

5. Vamos a coger un arquetipo de ejemplo: *maven-arquetype-quickstart*

6. Ahora tenemos que insertar la información del proyecto (Artifact Coordinates)

   - *GroupId* : com.techuniversity
   - *ArtifactId* : demo-proyecto-maven
   - *Version*: 1.0.0

7. Abrir el archivo de configuración del proyecto **pom.xml** y observar su contenido.

   (**img1.png**)

   

   Referencias:

   https://maven.apache.org/guides/introduction/introduction-to-archetypes.html

   https://javaspringvaadin.wordpress.com/2018/05/22/mavenintellijidea/