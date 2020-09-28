### LTEX14 - Estilado básico con bootstrap, componente test-bootstrap - 30 min

Vamos a crear un componente en el que enlazaremos Bootstrap, un 
	framework CSS para facilitar el proceso de estilado, y haremos 
	pruebas básicas de estilo.

Obtendremos Bootstrap en https://getbootstrap.com/ 

Creamos el componente test-bootstrap siguiendo el proceso habitual.

test-bootstrap.js

```javascript
import { LitElement, html } from 'lit-element';

class TestBootstrap extends LitElement {
	
	static get properties() {
		return {			
		};
	}
	
	constructor() {
		super();			
	}
	
	render() {
		return html`
			<!-- Link Bootstrap here -->
			<h3>Test Bootstrap</h3>
		`;
	}    
}

customElements.define('test-bootstrap', TestBootstrap)
```

Lo importamos y añadimos en la página principal de nuestra aplicación 
	(para esta prueba es más cómodo estar viendo solo test-bootstrap):

index.html

```html
<head>
  <!-- Other head stuff →
	<script type="module" src="./src/test-bootstrap/test-bootstrap.js" ></script>	
</head>
<body>    
	<test-bootstrap></test-bootstrap>
</body>
```

Importamos css y añadimos unos estilos de prueba al componente.

test-bootstrap.js

```javascript
import { LitElement, html, css } from 'lit-element';

class TestBootstrap extends LitElement {
	
	// Properties
	
	static get styles() {
		return css`	  	
			.redbg {
				background-color: red;
			}
		
			.greenbg {
				background-color: green;
			}
		
			.bluebg {
				background-color: blue;
			}
		
			.greybg {
				background-color: grey;
			}
		`;
	}
	
	// Constructor and render
}

customElements.define('test-bootstrap', TestBootstrap)
```

Los probamos: creamos una estructura HTML para dividir el componente 
	en columnas, que dividirán el espacio en partes iguales.

test-bootstrap.js

```javascript
// Modify only the render function.

render() {
    return html`	
		<!-- Bootstrap link -->
		<h3>Test Bootstrap</h3>
		<div class="row greybg">
			<div class="col redbg">Col 1</div>
			<div class="col greenbg">Col 2</div>
			<div class="col bluebg">Col 3</div>
		</div>
	`;
}
```

También se puede asignar un ancho a las columnas.

El ancho máximo es 12.

Si se supera este ancho máximo, se hace un salto.

test-bootstrap.js

```javascript
// Modify only the render function.

render() {
    return html`	
		<!-- Bootstrap link →
		<h3>Test Bootstrap</h3>
		<div class="row greybg">
			<div class="col-2 redbg">Col 1</div>
			<div class="col-3 greenbg">Col 2</div>
			<div class="col-4 bluebg">Col 3</div>
		</div>
	`;
}
```

Se puede aplicar más de un selector a las columnas.

Con estos selectores se precisan los diseños responsive.

Cuando esté activo se aplicará a ese y a los mayores.

Podemos probarlo haciendo más grande o pequeña la ventana del navegador,
	veremos que cambia el ancho de las columnas.

test-bootstrap.js

```javascript
// Modify only the render function.

render() {
    return html`	
		<!-- Bootstrap link →
		<h3>Test Bootstrap</h3>
		<div class="row greybg">
			<div class="col-2 col-sm-6 redbg">Col 1</div>
			<div class="col-3 col-sm-1 greenbg">Col 2</div>
			<div class="col-4 col-sm-1 bluebg">Col 3</div>
		</div>
	`;
}
```

También podemos aplicar un desplazamiento a las columnas.

test-bootstrap.js

```javascript
// Modify only the render function.

render() {
    return html`	
		<!-- Bootstrap link →
		<h3>Test Bootstrap</h3>
		<div class="row greybg">
			<div class="col-2 offset-1 redbg">Col 1</div>
			<div class="col-3 offset-2 greenbg">Col 2</div>
			<div class="col-4 bluebg">Col 3</div>
		</div>
	`;
}
```
