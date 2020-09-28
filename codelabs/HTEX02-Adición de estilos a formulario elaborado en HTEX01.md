summary: Adición de estilos a formulario elaborado en HTEX01
id: HTEX02
categories: HTML
tags: front
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# HTEX02 - Adición de estilos a formulario elaborado en HTEX01
<!-- ------------------------ -->
## Overview 
Duration: 15

### HTEX02 - Adición de estilos a formulario elaborado en HTEX01 - 15 min

Partiremos de la página realizada en el ejercicio anterior.

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

En el código de la página utilizamos las **etiquetas** HTML para indicar 
la maquetación de la página.

Se pueden añadir, como hemos visto, atributos id a los elementos.

Además, también se puede añadir el atributo **class**, que puede contener uno 
o varios valores, que sirvan para luego **seleccionar** ese elemento y aplicarle 
estilos.

Por ejemplo, un input de texto con id y class.

```html
<input id="formNombre" class="centrado caps otraClase" type="text" required placeholder="Nombre" />
```

CSS nos permite aplicar estilos al HTML de una página, pudiendo cambiar por 
ejemplo el color de una tipografía, su tamaño o centrar un componente.

Para aplicar estos estilos necesitamos alguna forma de poder **seleccionar** 
a qué partes del HTML aplicarlos y ahí es donde entran los **selectores** CSS,
que utilizarán los mecanismos que acabamos de ver antes.

Si queremos seleccionar en base a una **etiqueta HTML** lo haríamos tal que:

```css
  input {
  }
```

Si queremos seleccionar en base a una **id** lo haríamos tal que:

```css
  #formNombre {
  }
```

Si queremos seleccionar en base a una **clase** lo haríamos tal que:

```css
  .centrado {
  }
```

Los selectores se pueden **combinar** de varias formas:

```css
  input .centrado {
  }
```


Vamos a ver los selectores en un ejemplo.

El objetivo será poner la tipografía de cada uno de los 3 inputs que tenemos 
en nuestro formulario de un color distinto, utilizando distintos selectores.

Para cambiar el color de la tipografía usaremos la propiedad **color**.

Con este selector, pondremos el texto de todos los inputs en rojo 
(podríamos poner solo input también).

```css
form input {
  color: red;
}
```

Ahora añadimos a un input, por ejemplo el email, la clase "green".

```css
<input id="formEmail" class="green" type="email" placeholder="email" />
```

Y CSS apropiado para seleccionar el HTML que tenga la clase "green" y poner 
el color de su tipografía verde.

```css
.green {
  color: green;
}
```

Por último, usaremos una de las id, la del input de texto del nombre por ejemplo, 
para seleccionarlo y poner el color de su tipografía azul.

```css
#formNombre {
  color: blue;
}
```

Y así nos quedaría cada input con el texto que contiene de un color distinto.

El HTML sería:

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
		<input id="formEmail" class="green" type="email" placeholder="email" />
		<br />
		<label>Edad</label>
		<input id="formAge" min="18" max="99" type="number"/>
		<br />		
        <button>Enviar</button>
    </form>
  </body>
</html>
```

Y el CSS

```css
form input {
  color: red;
}

.green {
  color: green;
}

#formNombre {
  color: blue;
}
```
