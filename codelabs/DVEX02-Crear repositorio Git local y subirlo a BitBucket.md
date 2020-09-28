summary: Crear repositorio Git local y subirlo a BitBucket
id: DVEX02
categories: DevOps
tags: front, back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# DVEX02 - Crear repositorio Git local y subirlo a BitBucket
<!-- ------------------------ -->
## Overview 
Duration: 10

#### DVEX02 - Crear repositorio Git local y subirlo a BitBucket

Cuando tengamos preparado el repositorio **remoto**, 
crearemos nuestro repositorio **local** que luego enviaremos al remoto.

NOTA - Los comandos están pensados para ejecutarse con Git Bash.

Para ello vamos a nuestro directorio de proyectos, creamos un nuevo 
directorio para alojar el repositorio, por ejemplo testreplocal,
entramos al directorio e inicializamos el repositorio con el siguiente comando:

```
git init
```

Lo que, si no hay error, tendría que inicializar el repositorio, apareciéndonos 
un mensaje de confirmación.

Para verificarlo y comprobar el estado del repositorio lo haremos con el comando:

```
git status
```

A continuación añadiremos contenido al directorio de nuestro repositorio. 
Según lo vayamos haciendo podemos ir comprobando con git status que 
Git va detectando lo que estamos añadiendo.

Empezaremos añadiendo un archivo nuevo, lo creamos con el comando:

```
touch inicial.txt
```

Creamos un nuevo directorio:

```
mkdir otrosarchivos
```

Entramos en el:

```
cd otrosarchivos
```

Y dentro creamos otro archivo:

```
touch otrofichero
```

Volvemos al directorio principal:

```
cd ..
```

Y añadimos nuestros cambios a staging en Git:

```
git add .
```

Confirmamos con git status que los cambios que están preparados para guardar 
son correctos y nos preparamos para hacer nuestro primer commit.

Configuramos los datos de nombre e email asociados al commit. Estos datos 
se mostrarán junto con los cambios que hagamos en la información del commit.

Para ello ponemos los comandos:

```
git config --global user.email correo@correo.com
```

Donde "correo@correo.com" sería la dirección de correo que queramos que aparezca 
en nuestro commit.

Y 

```
git config --global user.name Nombre
```

Donde Nombre sería el nombre que queramos que aparezca en nuestro commit.

Ahora hacemos el commit en si con el comando:

```
git commit -m "Versión inicial"
```

Nos tendría que aparecer un mensaje indicando que el commit ha funcionado.

Comprobamos con **git status** el estado del repositorio.

Ahora vamos a modificar el archivo inicial.txt con un editor, salvar el cambio 
y comprobar con **git status** que Git detecta el cambio.

Una vez tengamos el cambio lo añadimos a staging:

```
git add .
```

Comprobamos con git status que el cambio preparado para guardar es correcto y 
hacemos el commit:

``` 
git commit -m "Inicial modificado"
```

Para finalizar, subiremos estos cambios que están en nuestro repositorio local 
al repositorio remoto en Bitbucket.

Para ello nos vamos primero a BitBucket y entramos en la página principal 
de nuestro repositorio. Podemos navegar por ejemplo haciendo click arriba a la 
izquierda en el logo de Bitbucket y seleccionar nuestro repositorio en la 
página que aparece. Una vez ahí, veremos que en la página aparece el texto:
"Get your local Git repository on Bitbucket".

Tal y como indica ahí, si no lo estamos ya, nos colocamos primero en el 
directorio principal de nuestro repositorio.

Para ver mejor lo que haremos a continuación, primero introduciremos el comando:

```
git remote -v
```

Este comando sirve para ver la configuración remota de nuestro repositorio, pero 
no nos saldrá nada, puesto que aun no hemos configurado este punto.

A continuación tenemos que ejecutar el comando que aparece en el paso 2
de la página de BitBucket, que será tal que:

```
git remote add origin URLDELREPOSITORIO.git
```

Y si ejecutamos de nuevo git remote -v veremos que hay configuraciones registradas.

Si esta parte ha funcionado, introduciremos el comando para subir el repositorio:

```
git push -u origin master
```

Nos pedirá nuestras credenciales de BitBucket y, si las introducimos correctamente
y va bien el proceso hará la subida al repositorio remoto.

Comprobamos con git status el estado del repositorio local y en la página del 
repositorio remoto en Bitbucket podremos ver y explorar los cambios.