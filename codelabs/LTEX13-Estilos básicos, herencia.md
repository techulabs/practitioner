### LTEX13 - Estilos básicos, herencia  - 15 min

Vamos a comprobar que hay unos estilos que son heredables, otros que no, 
	que esta herencia pasa a los componentes y que podemos manejar esto.
	
Vamos a empezar añadiendo un estilo de prueba en nuestra página principal.

index.html

```html
<head>
  <!-- Other head stuff -->
  <style>
  
	<!-- Other styles -->
    .test {
      color: red;
    }
  </style>
</head>

<body class="test">
  <!-- Body content -->
</body>
```

El estilo "color" indica el color de la tipografia. Como está puesto en el 
	body, veremos que las etiquetas nos salen en color rojo en todo.
	Este estilo es heredable.
	
Si dentro de uno de los componentes, por ejemplo persona-main.js, añadimos 
estilos propios, podríamos manejar esto.

persona-main.js

```javascript
// Add css to import too.
import { LitElement, html, css } from 'lit-element';

// Add this function to the component.
static get styles() {
	return css`	  
		:host {					
			all: initial;
		}
	`;
}
```

Con este código no veremos en rojo el texto en persona-main.

Por último, podemos comprobar que otros estilos, como por ejemplo border, 
	no son heredables. Para ello cambiamos el estilo en la página principal 
	tal que:

index.html

```html
<head>
  <!-- Other head stuff -->
  <style>
  
	<!-- Other styles -->
    .test {
      border: solid red;
    }
  </style>
</head>
```

Y vemos que solo se aplica el borde al body.