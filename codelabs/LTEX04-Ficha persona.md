summary: Ficha persona
id: LTEX04
categories: LitElement
tags: front
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# LTEX04 - Ficha persona
<!-- ------------------------ -->
## Overview 
Duration: 45

### LTEX04 - Ficha persona - 45 min

Vamos a crear el primer componente de nuestra app: ficha-persona.

Partiremos del componente hola-mundo creado previamente y:

· Duplicaremos el directorio del componente hola-mundo, renombrando el 
	nuevo directorio a ficha-persona.
· Dentro del directorio ficha-persona, seguirá habiendo un archivo 
	hola-mundo.js, renombraremos ese archivo a ficha-persona.js

Editamos el archivo ficha-persona.js y:

· Cambiamos el class HolaMundo por class FichaPersona.
· Cambiamos el texto en el div por Ficha Persona.
· Cambiamos el customElements.define('hola-mundo', HolaMundo)
	por customElements.define('ficha-persona', FichaPersona)

En este momento el archivo ficha-persona.js será:

ficha-persona.js

```javascript
import { LitElement, html } from 'lit-element';

class FichaPersona extends LitElement {
  render() {
    return html`
		<div>Ficha Persona</div>
	`;
  }
}

customElements.define('ficha-persona', FichaPersona)
```

Lo importamos en el index.html y lo añadimos a la plantilla 
para comprobar que funcione.

Una vez hecha la comprobación, añadimos las propiedades del componente.

ficha-persona.js

```javascript
import { LitElement, html } from 'lit-element';

class FichaPersona extends LitElement {	
	
	static get properties() {
		return {
			name: {type: String},			
			yearsInCompany: {type: Number},			
			photo: {type: Object}			
		};
	}			  
	
	// Rest of component's code
}
```

Y la plantilla del componente, para recoger el valor de las propiedades 
	(usar la ruta a la imagen que se considere 
		oportuna para la persona).

```javascript
import { LitElement, html } from 'lit-element';

class FichaPersona extends LitElement {

  // Properties code up here	
  render() {
    return html`
		<div>
			<label for="fname">Nombre Completo</label>
			<input type="text" id="fname" name="fname"></input>
			<br />						
			<label for="yearsInCompany">Años en la empresa</label>
			<input type="text" name="yearsInCompany"></input>
			<br />			
			<img src="./img/persona.jpg" height="200" width="200" alt="Foto persona">
		</div>
	`;
  }
  // Rest of component's code
}

customElements.define('ficha-persona', FichaPersona)
```

El componente completo quedaría tal que:

```javascript
import { LitElement, html } from 'lit-element';

class FichaPersona extends LitElement {
	
	static get properties() {		
		return {			
			name: {type: String},			
			yearsInCompany: {type: Number},			
			photo: {type: Object}			
		};
	}			  	
	
	render() {
		return html`
			<div>
				<label for="fname">Nombre Completo</label>
				<input type="text" id="fname" name="fname"></input>
				<br />						
				<label for="yearsInCompany">Años en la empresa</label>
				<input type="text" name="yearsInCompany"></input>
				<br />			
				<img src="./img/persona.jpg" height="200" width="200" alt="Foto persona">			
			</div>
		`;
	}
}

customElements.define('ficha-persona', FichaPersona)
```

### 