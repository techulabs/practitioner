summary: Crear cuenta Bitbucket y repositorio remoto
id: DVEX01
categories: DevOps
tags: front, back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# DVEX01 - Crear cuenta Bitbucket y repositorio remoto
<!-- ------------------------ -->
## Overview 
Duration: 10

#### DVEX01 - Crear cuenta Bitbucket y repositorio remoto

Vamos a crear un repositorio Git en nuestro entorno local y luego 
subiremos el código a Bitbucket.

Empezaremos yendo a la página oficial de Bitbucket https://bitbucket.org/ 
y creando ahí una cuenta usando para ello el correo personal, no el corporativo.

Una vez esté la cuenta creada, accedemos a ella y creamos un proyecto. 

Para ello, en el menú de la izquierda pulsamos el + y seleccionamos 
	 "Project" del menú de creación.
	 
Datos del proyecto:
	· Nombre - Prueba 
	· Key - Se rellenará según el nombre, PRUEB.
	· Descripción - Proyecto de prueba.
	· Privacy - Desmarcar el checkbox para que sea público.
	
Y pulsamos el botón para crear el proyecto.

A continuación vamos a crear un nuevo repositorio. 

Podremos seleccionar directamente en la pantalla que aparece tras crear el 
proyecto el botón "Create repository" o bien pulsar el + a la izquierda 
de nuevo y seleccionar "Repository".

Datos del repositorio:
	· Proyecto - Seleccionar Prueba.
	· Nombre - repoprueba
	· Access Level - Desmarcar el checkbox para que sea público.
	· Include a README - No.
	· Include .gitignore - No.
	
Y pulsamos el botón para crear el repositorio.


NOTA - En esta primera creación del repositorio no hemos añadido un archivo .gitignore pero, cuando trabajemos con nuestra app del curso, el repositorio que manejemos sí tendrá que tener un archivo .gitignore que indique, al menos (puede ser que según cada caso haya que añadir más), que queremos que Git ignore el directorio node_modules, que contiene las dependencias instaladas por npm.

El contenido del fichero sería:
	
	.gitignore
	```
	node_modules/
	```
