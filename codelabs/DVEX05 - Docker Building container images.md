summary: Docker: Building container images
id: DVEX05
categories: DevOps
tags: back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# DVEX04 - Docker: Building container images
<!-- ------------------------ -->
## Overview 
Duration: 45

**DVEX05 - Docker: Building container images**

En este codelab veremos los pasos para construir y lanzar un **contenedor Docker.** En concreto, el contenedor lanzará una página web HTML mediante Nginx, un servidor web alto rendimiento.

Las imágenes de Docker se crean basándose en un **Dockerfile**. Un Dockerfile define todos los pasos necesarios para crear una imagen de Docker con su aplicación configurada y lista para ejecutarse como contenedor. La imagen en sí contiene todo, desde el sistema operativo hasta las dependencias y la configuración necesaria para ejecutar su aplicación.

1. Comprobar la versión de Docker: 

   `docker -v`

2. Listar las imágenes locales:

   `docker images`

3. Crea el archivo **Dockerfile** ubicado en la carpeta raíz del proyecto:

   ```dockerfile
   FROM nginx:1.11-alpine
   COPY index.html /usr/share/nginx/html/index.html
   EXPOSE 80
   CMD ["nginx", "-g", "daemon off;"]
   ```

4. A partir del Dockerfile creado en el paso anterior vamos a construir la **imagen** *my-nginx-image* con la etiqueta versión 'latest':

   `docker build -t my-nginx-image:latest .`

5. Comprobamos ahora que la imagen se ha creado en local:

   `docker images`

6. Ahora ya podemos lanzar un contenedor a partir de la imagen creada en el paso 3:

   `docker run -d -p 80:80 my-nginx-image:latest`

7. Listar contenedores en ejecución:

   `docker ps`

8. Si estamos usando una terminal bash podremos comprobar la ejecución del servido web con el comando curl:

   `curl -i http://docker`

9. Detenemos el contenedor:

   `docker stop <container_id>`

10. Lo podemos relanzar con:

    `docker start <container_id>`

11. Eliminar la imagen:

    `docker rmi <image_id>`



HERRAMIENTA ONLINE DOCKER

https://www.katacoda.com/courses/docker/2

REFERENCIAS

- https://spring.io/guides/gs/spring-boot-docker/   (RETO)
- https://www.katacoda.com/courses/docker/deploying-first-container