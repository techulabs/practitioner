### LTEX09 - Práctica composición de plantillas: estructura básica aplicación - 30 min 

Vamos a crear una estructura compuesta de plantillas 
en nuestra aplicación. Tendremos un componente principal, app, 
que contendrá al resto:
	· Header
	· Main
	· Footer
	
Primero creamos el componente principal persona-app:
· Crear nuevo directorio persona-app
· Crear archivo js persona-app.js (duplicar otro o crear de cero).

En el index.html, importar y añadir a la plantilla el nuevo componente 
	persona-app (es preferible dejar en el index.html mostrándose 
	solo persona-app para mayor claridad).

Y creamos el resto de componentes, que iremos añadiendo a 
	la plantilla de persona-app.

El código de los componentes y de persona-app sería:


persona-header.js

```javascript
import { LitElement, html } from 'lit-element';

class PersonaHeader extends LitElement {
	
	static get properties() {
		return {			
		};
	}
	
	constructor() {
		super();			
	}
	
	render() {
		return html`
		<h1>App molona</h1>
		`;
	}    
}

customElements.define('persona-header', PersonaHeader)
```

persona-main.js

```javascript
import { LitElement, html } from 'lit-element';

class PersonaMain extends LitElement {
	
	static get properties() {
		return {			
		};
	}
	
	constructor() {
		super();			
	}
	
	render() {
		return html`
			<h2>Main</h2>
		`;
	}    
}

customElements.define('persona-main', PersonaMain)
```

persona-footer.js

```javascript
import { LitElement, html } from 'lit-element';

class PersonaFooter extends LitElement {
	
	static get properties() {
		return {			
		};
	}
	
	constructor() {
		super();			
	}
	
	render() {
		return html`
			<h5>@PersonaApp 2020</h5>
	`;
  }    
}

customElements.define('persona-footer', PersonaFooter)
```

```javascript
import { LitElement, html } from 'lit-element';
import '../persona-header/persona-header.js';
import '../persona-main/persona-main.js';
import '../persona-footer/persona-footer.js';

class PersonaApp extends LitElement {
	
	static get properties() {
		return {			
		};
	}
	
	constructor() {
		super();			
	}
	
	render() {
		return html`
			<persona-header></persona-header>
			<persona-main></persona-main>
			<persona-footer></persona-footer>
		`;
	}    
}

customElements.define('persona-app', PersonaApp)
```
