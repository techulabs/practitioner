summary: Clonar repositorio Git esqueleto y mover a repositorio propio
id: DVEX03
categories: DevOps
tags: front, back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# DVEX03 - Clonar repositorio Git esqueleto y mover a repositorio propio
<!-- ------------------------ -->
## Overview 
Duration: 10

#### DVEX03 - Clonar repositorio Git esqueleto y mover a repositorio propio

Vamos a ver cómo trabajar con un repositorio ya creado que añadiremos a uno 
creado por nosotros.

El objetivo será clonar un repositorio ya existente a nuestra máquina local y 
después enviarlo a uno remoto creado por nosotros.

En nuestro directorio de proyectos, creamos un nuevo directorio para alojar 
el repositorio remoto que clonaremos y entramos en ese directorio.

Estando en el nuevo directorio clonamos el repositorio remoto con el comando:

```
git clone https://oyara@bitbucket.org/oyara/esqueleto.git
```

Entramos en el directorio que ha creado, esqueleto, y ahí ejecutamos 
git status para comprobar el estado del repositorio.

Si ponemos git remote -v veremos que este repositorio, como ya estaba creado,
tiene su propia configuración ya puesta.

Nuestro objetivo aquí es modificar esa configuración para enviar este repositorio 
que hemos clonado a uno remoto creado por nosotros.

Lo que haremos será quitar este enlace a origin para cuando añadamos el nuestro, 
para ello usamos el comando:

```
git remote rm origin
```

Y ahora si ponemos git remote -v de nuevo veremos que volvemos a no tener 
configuración.

Para continuar nos vamos a Bitbucket y creamos un nuevo repositorio 
siguiendo los pasos del ejercicio DVEX01.

Una vez lo tengamos creado, en la página principal del repositorio, y tal 
y está indicado en el ejercicio DVEX02, tenemos que coger los comandos de
"Get your local Git repository on Bitbucket", paso 2, el comando con formato 
tal que:

```
git remote add origin URLDELREPO.git
```

Y ejecutarlo. Si va bien el cambio al ejecutar de nuevo git remote -v 
tendríamos que ver la nueva configuración.

Nos resta solo hacer el push tal y como indica la referencia en Bitbucket y 
hemos visto en el ejercicio DVEX02 y comprobar en BitBucket que el repositorio 
ha quedado subido.