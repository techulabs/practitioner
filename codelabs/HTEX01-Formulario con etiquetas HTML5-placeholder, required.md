summary: Formulario con etiquetas HTML5 - placeholder, required
id: HTEX01
categories: HTML
tags: front
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# HTEX01 - Formulario con etiquetas HTML5 - placeholder, required
<!-- ------------------------ -->
## Overview 
Duration: 30

### HTEX01 - Formulario con etiquetas HTML5 - placeholder, required - 30 min

Vamos a crear una página básica HTML en la que probaremos etiquetas y atributos 
de interés.

El ejercicio se puede hacer en la página https://jsfiddle.net/ .

Empezaremos creando la página HTML tal que:

```html
<!doctype html>
<html>
  <head>
  </head>
  <body>
    <h1>¡Hola Mundo!</h1>
  </body>
</html>
``` 

Añadimos un título a la página. 

Añadimos también un formulario y un input de texto, 
al que añadimos una id para identificarlo.

Completamos el formulario añadiendo un botón para enviarlo.

```html
<!doctype html>
<html>
  <head>
    <title>This is a test</title>
  </head>
  <body>
    <h1>¡Hola Mundo!</h1>
    <form>
        <input id="formNombre" type="text"/>
        <br />
        <button>Enviar</button>
    </form>
  </body>
</html>
``` 

En este punto podemos comprobar que, si le damos al botón enviar, se 
envía el formulario, tenga o no valor el input de texto.

Si añadimos al input del formulario el atributo **required**, no podremos 
enviarlo a no ser que le demos un valor.

```html
<!doctype html>
<html>
  <head>
    <title>This is a test</title>
  </head>
  <body>
    <h1>¡Hola Mundo!</h1>
    <form>
        <input id="formNombre" type="text" required />
        <br />
        <button>Enviar</button>
    </form>
  </body>
</html>
``` 

El atributo **placeholder** nos ayuda a identificar qué ha de contener 
un input de texto.

```html
<!doctype html>
<html>
  <head>
    <title>This is a test</title>
  </head>
  <body>
    <h1>¡Hola Mundo!</h1>
    <form>
        <input id="formNombre" type="text" required placeholder="Nombre" />
        <br />
        <button>Enviar</button>
    </form>
  </body>
</html>
```

Hay tipos de input más específicos, como por ejemplo email, que añaden 
una validación previa al envío, comprobando que el contenido cumpla un 
patrón predeterminado.

```html
<!doctype html>
<html>
  <head>
    <title>This is a test</title>
  </head>
  <body>
    <h1>¡Hola Mundo!</h1>
    <form>
        <input id="formNombre" type="text" required placeholder="Nombre" />
        <br />
		<input id="formEmail" type="email" placeholder="email" />
		<br />
        <button>Enviar</button>
    </form>
  </body>
</html>
```

Existe también el input de tipo **number** que cuenta con atributos 
específicos, min y max, para delimitar los números permitidos.

```html
<!doctype html>
<html>
  <head>
    <title>This is a test</title>
  </head>
  <body>
    <h1>¡Hola Mundo!</h1>
    <form>
        <input id="formNombre" type="text" required placeholder="Nombre" />
        <br />
		<input id="formEmail" type="email" placeholder="email" />
		<br />
		<input id="formAge" min="18" max="99" type="number"/>
		<br />
        <button>Enviar</button>
    </form>
  </body>
</html>
```

También podemos añadir etiquetas a los input para mejorar su usabilidad.

```html
<!doctype html>
<html>
  <head>
    <title>This is a test</title>
  </head>
  <body>
    <h1>¡Hola Mundo!</h1>
    <form>
		<label>Nombre</label>
        <input id="formNombre" type="text" required placeholder="Nombre" />
        <br />
		<label>Email</label>
		<input id="formEmail" type="email" placeholder="email" />
		<br />
		<label>Edad</label>
		<input id="formAge" min="18" max="99" type="number"/>
		<br />
        <button>Enviar</button>
    </form>
  </body>
</html>
```


Existen más tipos de input, como por ejemplo **date** y **color**.

```html
<!doctype html>
<html>
  <head>
    <title>This is a test</title>
  </head>
  <body>
    <h1>¡Hola Mundo!</h1>
    <form>
		<label>Nombre</label>
        <input id="formNombre" type="text" required placeholder="Nombre" />
        <br />
		<label>Email</label>
		<input id="formEmail" type="email" placeholder="email" />
		<br />
		<label>Edad</label>
		<input id="formAge" min="18" max="99" type="number"/>
		<br />
		<input id="formDate" type="date"/>
		<br />
		<input id="formColor" type="color"/>
		<br />
        <button>Enviar</button>
    </form>
  </body>
</html>
```
