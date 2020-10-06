summary: Prueba evento y función manejadora
id: LTEX07
categories: LitElement
tags: front
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# LTEX07 - Prueba evento y función manejadora
<!-- ------------------------ -->
## Overview 
Duration: 15

### LTEX07 - Prueba evento y función manejadora

Podemos capturar eventos lanzados por las interacciones del 
usuario con la página y nuestros componentes.

Capturamos el evento @input en el input de texto del nombre.

ficha-persona.js

```javascript
<input type="text" id="fname" name="fname" value="${this.name}" @input="${this.updateName}"></input>
```

Asociaremos al evento una función manejadora, en la que llevaremos 
	a cabo los cambios oportunos según el evento.

Podemos usar estas funciones para variar el valor de propiedades del componente.

La función la colocaremos dentro del ámbito de la clase del componente.

ficha-persona.js

```javascript
updateName(e) {
	console.log("updateName");
	this.name = e.target.value;	  
}
```

Ahora, cuando cambie el valor del input de texto, podremos ver en los logs 
	que el valor de la propiedad se actualiza también.

El código completo del componente sería.

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
				<input type="text" id="fname" name="fname" value="${this.name}" @input="${this.updateName}"></input>
				<br />						
				<label for="yearsInCompany">Años en la empresa</label>
				<input type="text" name="yearsInCompany" value="${this.yearsInCompany}"></input>
				<br />			
				<img src="${this.photo.src}" height="200" width="200" alt="${this.photo.alt}">
			</div>
		`;
	}
		
	updated(changedProperties) {	   
		if (changedProperties.has("name")) {
			console.log("Propiedad name cambiada valor anterior era " + changedProperties.get("name") + " nuevo es " + this.name);
		}	
	}
	
	updateName(e) {
		console.log("updateName");
		this.name = e.target.value;	  
	}
}

customElements.define('ficha-persona', FichaPersona)
```
