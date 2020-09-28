### LTEX16 - Práctica base eventos - emisor, receptor, gestor 120 min

En este ejercicios veremos las bases de los eventos nativos y custom,
	que nos permitirán pasar datos entre componentes.

Los componentes se comunican con el exterior lanzando eventos y 
	enviando datos en ellos.

Los componentes se pueden modificar desde el exterior cambiando 
	el valor de sus propiedades.

En el ejercicio veremos el paso de datos en los dos sentidos:

· Partiremos de un componente **emisor-evento**, que 
	lanzará un evento con datos.
· Este evento será recogido por otro componente, **gestor-evento**, 
	que actuará de mediador.
· Por último, los datos que ha recogido **gestor-evento**, 
	se enviarán a otro componente **receptor-evento** 
	que los mostrará en su plantilla.
	
	
Empezamos creando el componente emisor evento de la forma habitual.

emisor-evento.js

```javascript
import { LitElement, html } from 'lit-element';

class EmisorEvento extends LitElement {
	
	static get properties() {
		return {			
		};
	}
	
	constructor() {
		super();			
	}
	
	render() {
		return html`	
			<h3>Emisor Evento</h3>
		`;
	}    
}

customElements.define('emisor-evento', EmisorEvento)
```

Lo enlazaremos en el index.html y lo añadiremos a la plantilla 
(será más sencillo de ver si es el único componente que se muestra).



index.html

```javascript
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

<script type="module" src="./src/emisor-evento/emisor-evento.js" ></script>

<body >
  <emisor-evento></emisor-evento>
</body>

</html>
```

Refactorizamos el código del emisor-evento:

· Añadimos un botón del que capturamos su evento click.
· En la función manejadora asociada al evento, lanzamos un 
	evento custom con datos.
· Los datos de este evento llegarán a gestor-evento y se 
	enviarán a receptor-evento.	

emisor-evento.js

```javascript
import { LitElement, html } from 'lit-element';

class EmisorEvento extends LitElement {
	
	// Other functions.
	
	render() {
		return html`	
			<h1>Emisor Evento</h1>
			<button @click="${this.sendEvent}">No pulsar</button>
		`;
	}
	
	sendEvent(e) {
		console.log("Pulsado el botón");
		console.log(e);
		
		this.dispatchEvent(
			new CustomEvent(
				"test-event",
				{
					"detail" : {
						"course" : "TechU",
						"year" : 2020
					}
				}
			)
		);
	}
}

customElements.define('emisor-evento', EmisorEvento)
```

Creamos el componente gestor-evento de la forma habitual.

gestor-evento.js

```javascript
import { LitElement, html } from 'lit-element';

class GestorEvento extends LitElement {
	
	static get properties() {
		return {			
		};
	}
	
	constructor() {
		super();
	}
	
	render() {
		return html`	
			<h1>Gestor Evento</h1>			
		`;
	}	
}

customElements.define('gestor-evento', GestorEvento)
```

Y lo enlazamos también en el index.html para probarlo 
	(quitamos el componente emisor-evento).


index.html

```javascript
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

<script type="module" src="./src/gestor-evento/gestor-evento.js" ></script>

<body >
  <gestor-evento></gestor-evento>
</body>

</html>
```

El componente gestor evento va a actuar como **mediador** entre los 
	componentes emisor evento y receptor evento.
	
Gestor evento contendrá ambos componentes en su plantilla.

Empezaremos añadiendo emisor evento a gestor evento. 

gestor-evento.js

```javascript
import { LitElement, html } from 'lit-element';
import '../emisor-evento/emisor-evento.js';

class GestorEvento extends LitElement {
	
	static get properties() {
		return {			
		};
	}
	
	constructor() {
		super();
	}
	
	render() {
		return html`	
			<h1>Gestor Evento</h1>
			<emisor-evento></emisor-evento>
		`;
	}	
}

customElements.define('gestor-evento', GestorEvento)
```

Una vez comprobamos que vemos los dos componentes, capturamos 
	el evento "test-event" de emisor-evento y añadimos una 
	función manejadora para procesarlo.

gestor-evento.js

```javascript
import { LitElement, html } from 'lit-element';
import '../emisor-evento/emisor-evento.js';

class GestorEvento extends LitElement {
	
	static get properties() {
		return {			
		};
	}
	
	constructor() {
		super();
	}
	
	render() {
		return html`	
			<h1>Gestor Evento</h1>
			<emisor-evento @test-event="${this.processEvent}"></emisor-evento>
		`;
	}
		
	processEvent(e) {
		console.log("Capturado evento del emisor");
		console.log(e.detail);
	}
}

customElements.define('gestor-evento', GestorEvento)
```

Creamos el componente receptor evento de la forma habitual.

receptor-evento.js

```javascript
import { LitElement, html } from 'lit-element';

class ReceptorEvento extends LitElement {
	
	static get properties() {
		return {			
		};
	}
	
	constructor() {
		super();			
	}
	
	render() {
		return html`	
			<h3>Receptor Evento</h3>			
		`;
	}		
}

customElements.define('receptor-evento', ReceptorEvento)
```

Y lo añadimos a gestor evento para probar que funciona.

gestor-evento.js

```javascript
import { LitElement, html } from 'lit-element';
import '../emisor-evento/emisor-evento.js';
import '../receptor-evento/receptor-evento.js';

class GestorEvento extends LitElement {
	
	static get properties() {
		return {			
		};
	}
	
	constructor() {
		super();
	}
	
	render() {
		return html`	
			<h1>Gestor Evento</h1>
			<emisor-evento @test-event="${this.processEvent}"></emisor-evento>
			<receptor-evento></receptor-evento>
		`;
	}
		
	processEvent(e) {
		console.log("Capturado evento del emisor");
		console.log(e.detail);
	}
}

customElements.define('gestor-evento', GestorEvento)
```

Añadimos a receptor evento propiedades para recoger los datos 
	que vienen en el evento lanzado por emisor evento.

Además, añadimos a la plantilla html y data binding para pintar 
	el valor de esas propiedades.
	
receptor-evento.js

```javascript
import { LitElement, html } from 'lit-element';

class ReceptorEvento extends LitElement {
	
	static get properties() {
		return {
			course: {type: String},
			year: {type: String}
		};
	}
	
	constructor() {
		super();			
	}
	
	render() {
		return html`	
			<h3>Receptor Evento</h3>		
			<h5>Este curso es de ${this.course}</h5>
			<h5>y estamos en el año ${this.year}</h5>
		`;
	}
}

customElements.define('receptor-evento', ReceptorEvento)
```

Desde la función manejadora del gestor, asignamos los valores 
	de las propiedades al receptor, al que hemos añadido 
	una id para seleccionarlo.

gestor-evento.js

```javascript
import { LitElement, html } from 'lit-element';
import '../emisor-evento/emisor-evento.js';
import '../receptor-evento/receptor-evento.js';

class GestorEvento extends LitElement {
	
	// Other functions.
	
	render() {
		return html`	
			<h1>Gestor Evento</h1>
			<emisor-evento @test-event="${this.processEvent}"></emisor-evento>
			<receptor-evento id="receiver"></receptor-evento>
		`;
	}
		
	processEvent(e) {
		console.log("Capturado evento del emisor");
		console.log(e.detail);
		
		this.shadowRoot.getElementById("receiver").course = e.detail.course;
		this.shadowRoot.getElementById("receiver").year = e.detail.year;
	}
}

customElements.define('gestor-evento', GestorEvento)
```

El código de los tres componentes quedaría:

gestor-evento.js

```javascript
import { LitElement, html } from 'lit-element';
import '../emisor-evento/emisor-evento.js';
import '../receptor-evento/receptor-evento.js';

class GestorEvento extends LitElement {
	
	static get properties() {
		return {			
		};
	}
	
	constructor() {
		super();
	}
	
	render() {
		return html`	
			<h1>Gestor Evento</h1>
			<emisor-evento @test-event="${this.processEvent}"></emisor-evento>
			<receptor-evento id="receiver"></receptor-evento>
		`;
	}
		
	processEvent(e) {
		console.log("Capturado evento del emisor");
		console.log(e.detail);
		
		this.shadowRoot.getElementById("receiver").course = e.detail.course;
		this.shadowRoot.getElementById("receiver").year = e.detail.year;
	}
}

customElements.define('gestor-evento', GestorEvento)
```

emisor-evento.js

```javascript
import { LitElement, html } from 'lit-element';

class EmisorEvento extends LitElement {
	
	static get properties() {
		return {			
		};
	}
	
	constructor() {
		super();			
	}
	
	render() {
		return html`	
			<h1>Emisor Evento</h1>
			<button @click="${this.sendEvent}">No pulsar</button>
		`;
	}
	
	sendEvent(e) {
		console.log("Pulsado el botón");
		console.log(e);
		
		this.dispatchEvent(
			new CustomEvent(
				"test-event",
				{
					"detail" : {
						"course" : "TechU",
						"year" : 2020
					}
				}
			)
		);
	}
}

customElements.define('emisor-evento', EmisorEvento)
```

receptor-evento.js

```javascript
import { LitElement, html } from 'lit-element';

class ReceptorEvento extends LitElement {
	
	static get properties() {
		return {
			course: {type: String},
			year: {type: String}
		};
	}
	
	constructor() {
		super();			
	}
	
	render() {
		return html`	
			<h3>Receptor Evento</h3>		
			<h5>Este curso es de ${this.course}</h5>
			<h5>y estamos en el año ${this.year}</h5>
		`;
	}
}

customElements.define('receptor-evento', ReceptorEvento)
```
