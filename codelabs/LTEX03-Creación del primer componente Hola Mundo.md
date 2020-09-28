summary: Creación del primer componente Hola Mundo
id: LTEX03
categories: LitElement
tags: front
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# LTEX03 - Creación del primer componente Hola Mundo
<!-- ------------------------ -->
## Overview 
Duration: 40

### LTEX03 - Creación del primer componente Hola Mundo - 40 min

Desde nuestro directorio de proyectos del curso, entramos en 
	el directorio de nuestra app.

```
cd base-litelement
```

Y vamos al directorio con el componente creado automáticamente.

```
cd src 
```

Ahí crearemos un directorio nuevo para nuestro componente

```
mkdir hola-mundo 
```

Entramos al directorio

```
cd hola-mundo
```

Y creamos el archivo del componente

```
touch hola-mundo.js
```

El código del componente será:

hola-mundo.js

```javascript
import { LitElement, html } from 'lit-element';

class HolaMundo extends LitElement {
  render() {
    return html`
		<div>Hola Mundo!</div>
	`;
  }
}

customElements.define('hola-mundo', HolaMundo)
```

En el directorio principal de la app, veremos un archivo index.html. Ese 
será el archivo que se cargue cuando lancemos nuestra app y ahí será 
donde añadiremos nuestro componente.

index.html

```html
<!doctype html>
<html lang="en">

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover" />
  <meta name="Description" content="Put your description here.">
  <base href="/">

  <style>
    html,
    body {
      margin: 0;
      padding: 0;
      font-family: sans-serif;
      background-color: #ededed;
    }
  </style>
  <title>base-litelement</title>
</head>

<script type="module" src="./src/hola-mundo/hola-mundo.js"></script>

<body>
  <hola-mundo></hola-mundo>
</body>

</html>
```

Ahora en la consola ponemos:

```
npm run start 
```

Lo que iniciará un servidor de pruebas y lanzará el navegador, en el que veremos 
nuestra app y el componente hola-mundo.