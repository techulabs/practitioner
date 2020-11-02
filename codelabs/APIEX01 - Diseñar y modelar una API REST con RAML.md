summary: Diseñar y modelar una API REST con RAML
id: APIEX01
categories: API
tags: back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# APIEX01 - Diseñar y modelar una API REST con RAML
<!-- ------------------------ -->
## Overview 
Duration: 110

##### APIEX01 - Diseñar y modelar una API REST con RAML

Vamos a crear un primer diseño en **RAML** (RESTful API Modeling Language) de una API básica BookMobile. 

Supongamos que te encargas del diseño de una API RESTful para una puesta en marcha de BookMobile. Has elaborado un plan de negocios y un plan de escala. Entonces, comencemos escribiendo una especificación.

## 1. Root

Las primeras líneas del fichero RAML (**.raml**) contendrán información básica.

```yaml
#%RAML 1.0
---
title: e-BookMobile API
baseUri: http://api.e-bookmobile.com/{version}
version: v1
```

Todo lo que ingrese en la raíz (o en la parte superior) de la especificación se aplica al resto de su API. Esto te resultará muy útil más adelante a medida que descubras patrones en la forma en que construyes tu API. La baseURI que elija se utilizará con cada llamada realizada, así que asegúrese de que sea lo más limpia y concisa posible.

## 2. Recursos

Todo lo que insertemos en la raíz (o en la parte superior) de la especificación se aplica al resto de su API. Esto te resultará muy útil más adelante a medida que descubras patrones en la forma en que construyes tu API. La baseURI que elijas se utilizará con cada llamada realizada, así que asegurate de que sea lo más simple y concisa posible.

```yaml
/users:
/authors:
/books:
```

Observa que todos estos recursos comienzan con una barra inclinada (/). En RAML, así es como se indica un recurso. Todos los métodos y parámetros anidados en estos recursos de nivel superior pertenecen a ese recurso y actúan sobre él. Ahora, dado que cada uno de estos recursos es una colección de objetos individuales (autores, libros y usuarios específicos), necesitaremos definir algunos sub-recursos para completar la colección.

Los recursos anidados son útiles cuando desea llamar a un subconjunto particular de su recurso para limitarlo. Por ejemplo:

```yaml
/authors:
  /{authorname}:
```

## 3. Métodos

Aquí es donde se decide lo que quiere que el desarrollador pueda hacer con los recursos que ha puesto a disposición. Puedes añadir tantos métodos como desees a cada recurso de su API BookMobile, en cualquier nivel. Sin embargo, cada método HTTP solo se puede utilizar una vez por recurso. 

En este ejemplo, queremos que los desarrolladores puedan trabajar a nivel de colección. Por ejemplo, sus consumidores de API pueden recuperar un libro de la colección (GET), agregar un libro (POST) o actualizar toda la biblioteca (PUT). No desea que puedan eliminar información al más alto nivel. Centrémonos en desarrollar el recurso / books.

Anida los métodos para permitir que los desarrolladores realicen estas acciones en sus recursos. Ten en cuenta que debe usar minúsculas para los métodos en su definición de API RAML:

```yaml
/books:
  get:
  post:
  put:
```

## 4. URI Parameters

Los recursos que definimos son colecciones de objetos relevantes más pequeños.  *bookTitle* es un parámetro de URI, denotado por llaves en RAML:

```yaml
/books:
  /{bookTitle}:
```

Entonces, para realizar una petición a este recurso anidado, la URI del libro de Mary Roach, Stiff se vería como *http: //api.e-bookmobile.com/v1/books/Stiff*

Es hora de editar su especificación para reflejar las características granulares inherentes de los recursos:

```yaml
/books:
  get:
  put:
  post:
  /{bookTitle}:
    get:
    put:
    delete:
    /author:
      get:
    /publisher:
      get:
```

## 5. Query parameters

Ahora, vamos a especificar los parámetros de consulta que van a permitir filtrar una colección. Empieza añadiendo algunos parámetros en el método GET para libros. 

```yaml
/books:
  get:
    queryParameters:
      author:
      publicationYear:
      rating:
      isbn:
  put:
  post:
```

Los querys parameters también pueden ser algo que el servidor requiera para procesar la solicitud del consumidor de API, como un token de acceso. Es normal, que se necesite una autorización de seguridad para modificar una colección o un documento.

Anida el query parameter del token de acceso bajo el método PUT para un título específico: 

```yaml
/books:
  /{bookTitle}
    get:
      queryParameters:
        author:
        publicationYear:
        rating:
        isbn:
    put:
      queryParameters:
        access_token:
```

Los recursos y métodos de una API suelen tener varios query parameters asociados. Cada parámetro de consulta puede tener cualquier número de atributos opcionales para definirlo mejor. Ahora, especificar los atributos para los query parameters que definimos anteriormente. 

```yaml
/books:
  /{bookTitle}
    get:
      queryParameters:
        author:
          displayName: Author
          type: string
          description: An author's full name
          example: Mary Roach
          required: false
        publicationYear:
          displayName: Pub Year
          type: number
          description: The year released for the first time in the US
          example: 1984
          required: false
        rating:
          displayName: Rating
          type: number
          description: Average rating (1-5) submitted by users
          example: 3.14
          required: false
        isbn:
          displayName: ISBN
          type: string
          minLength: 10
          example: 0321736079
    put:
      queryParameters:
        access_token:
          displayName: Access Token
          type: string
          description: Token giving you permission to make call
          required: true
```

Para realizar una llamada PUT, la URI deberá ser: 

`http: //api.e-bookmobile.com/books/Stiff? Access_token = ACCESS * TOKEN`

## 7. Responses

Las respuestas deben contener uno más códigos de estado HTTP, y puede incluir descripciones, ejemplos o esquemas.

```yaml
/books:
  /{bookTitle}:
    get:
      description: Retrieve a specific book title
      responses:
        200:
          body:
            application/json:
              example: |
                {
                  "data": {
                    "id": "SbBGk",
                    "title": "Stiff: The Curious Lives of Human Cadavers",
                    "description": null,
                    "datetime": 1341533193,
                    "genre": "science",
                    "author": "Mary Roach",
                    "link": "http://e-bookmobile.com/books/Stiff"
                  },
                  "success": true,
                  "status": 200
                }
```

¡Enhorabuena! Acabas de completar la definición de un API en RAML.

REFERENCIAS:

- https://raml.org/developers/raml-100-tutorial#
- https://platform.bbva.com/en-us/developers/bbva-catalog/apis/documentation/raml/defining-an-api-with-raml
- https://platform.bbva.com/en-us/developers/bbva-catalog/apis/documentation/examples/raml-code-examples