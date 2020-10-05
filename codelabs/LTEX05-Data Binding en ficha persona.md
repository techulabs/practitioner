summary: Data Binding en ficha persona
id: LTEX05
categories: LitElement
tags: front
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# LTEX05 - Data Binding en ficha persona
<!-- ------------------------ -->
## Overview 
Duration: 25

### LTEX05 - Data Binding en ficha persona

Vamos a añadir un data binding básico a nuestro componente.

Añadimos los data binding en la plantilla:

ficha-persona.js

```javascript
import { LitElement, html } from 'lit-element';

class FichaPersona extends LitElement {

  // Properties code up here	
  render() {
    return html`
		<div>
			<label for="fname">Nombre Completo</label>
			<input type="text" id="fname" name="fname" value="${this.name}"></input>
			<br />						
			<label for="yearsInCompany">Años en la empresa</label>
			<input type="text" name="yearsInCompany" value="${this.yearsInCompany}"></input>
			<br />			
			<img src="${this.photo.src}" height="200" width="200" alt="${this.photo.alt}">			
		</div>
	`;
  }
  // Rest of component's code
}

customElements.define('ficha-persona', FichaPersona)
```

E inicializaremos los valores de las propiedades en el constructor
	(no olvidar llamar siempre a super() lo primero).

ficha-persona.js

```javascript
// Properties up here

constructor() {
	// Always calls super() first.
	super();
	
	this.name = "Prueba Nombre";
	this.yearsInCompany = 12;
	this.photo = {
		src: "./img/persona.jpg",
		alt: "foto persona"			
	};			
}
	
// Render and rest of code down here.
```

Ahora, al cargar de nuevo, tendríamos que ver 
	los valores en los inputs de texto.

El componente quedaría tal que:

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
	
	constructor() {
		// Always calls super() first.
		super();
		
		this.name = "Prueba Nombre";		
		this.yearsInCompany = 12;
		this.photo = {
			src: "./img/persona.jpg",
			alt: "foto persona"			
		};			
	}
			
	render() {
		return html`
			<div>
				<label for="fname">Nombre Completo</label>
				<input type="text" id="fname" name="fname" value="${this.name}"></input>
				<br />						
				<label for="yearsInCompany">Años en la empresa</label>
				<input type="text" name="yearsInCompany" value="${this.yearsInCompany}"></input>
				<br />			
				<img src="${this.photo.src}" height="200" width="200" alt="${this.photo.alt}">
			</div>
		`;
	}
}

customElements.define('ficha-persona', FichaPersona)
```
