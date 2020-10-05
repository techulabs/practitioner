summary: Prueba updated en propiedades
id: LTEX06
categories: LitElement
tags: front
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# LTEX06 - Prueba updated en propiedades
<!-- ------------------------ -->
## Overview 
Duration: 10

### LTEX06 - Prueba updated en propiedades

Como parte del ciclo de vida de un componente Lit Element, podemos 
	usar funciones que nos den información de interés 
	para poder reaccionar ante cambios.


Por ejemplo, la función **updated** se 
	llama cuando se ha actualizado y pintado el DOM del componente.

Podríamos ver si en esa actualización está la propiedad "name".

ficha-persona.js

```javascript
// changedProperties is a Map.
updated(changedProperties) {	   
	if (changedProperties.has("name")) {
		console.log("Propiedad name cambiada anterior era " + changedProperties.get("name") + " nuevo es " + this.name);
	}	
}
```

Si consultamos los logs en la consola del navegador, podremos ver el 
cambio inicial de name de undefined al valor que hayamos 
definido en el constructor.

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
		
	updated(changedProperties) {	   
		if (changedProperties.has("name")) {
			console.log("Propiedad name cambiada valor anterior era " + changedProperties.get("name") + " nuevo es " + this.name);
		}	
	}
}

customElements.define('ficha-persona', FichaPersona)
```
