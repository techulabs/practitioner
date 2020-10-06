summary: Propiedad personInfo
id: LTEX08
categories: LitElement
tags: front
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# LTEX08 - Propiedad personInfo
<!-- ------------------------ -->
## Overview 
Duration: 45

### LTEX08 - Propiedad personInfo

En este ejercicio vamos a añadir una propiedad, cuyo valor mostraremos en la plantilla, 
	con la particularidad de que el valor lo calcularemos en base al valor de otra 
	propiedad del componente.
	
Este valor se calculará inicialmente en el constructor del componente y, además, 
	cada vez que se varíe el valor de la propiedad en base a la cual se calcula, 
	habrá que calcular el valor de nuevo. Esto lo haremos usando eventos y la función 
	updated, del ciclo de vida de LitElement.
	
	
Comenzamos añadiendo la propiedad personInfo al componente.

ficha-persona.js

```javascript
static get properties() {		
	return {			
		name: {type: String},			
		yearsInCompany: {type: Number},			
		personInfo: {type: String},
		photo: {type: Object}			
	};
}			  	
```

Calcularemos su valor en una función, puesto que haremos este proceso en 
el momento de cargar el componente y cuando cambie el valor de la propiedad 
en base a la cual sabremos el valor de personInfo. De esta forma, podremos 
reutilizar el código para calcular el valor.


ficha-persona.js

```javascript
updatePersonInfo() {

	console.log("updatePersonInfo");
	console.log("yearsInCompany vale " + this.yearsInCompany);
	
	if (this.yearsInCompany >= 7) {
		this.personInfo = "lead";
	} else if (this.yearsInCompany >= 5) {
		this.personInfo = "senior";
	} else if (this.yearsInCompany >= 3) {
		this.personInfo = "team";
	} else {
		this.personInfo = "junior";
	}
}
```

Llamaremos a esta función en el constructor.

ficha-persona.js

```javascript
constructor() {
	// Always calls super() first.
	super();
		
	this.name = "Prueba Nombre";		
	this.yearsInCompany = 12;
	this.photo = {
		src: "./img/persona.jpg",
		alt: "foto persona"			
	};			
	
	this.updatePersonInfo();
}
```

Y mostraremos el valor de la propiedad en la plantilla (deshabilitando 
	la edición):

ficha-persona.js

```javascript
render() {
	return html`
		<div>
			<label for="fname">Nombre Completo</label>
			<input type="text" id="fname" name="fname" value="${this.name}" @input="${this.updateName}"></input>
			<br />						
			<label for="yearsInCompany">Años en la empresa</label>
			<input type="text" name="yearsInCompany" value="${this.yearsInCompany}"></input>
			<br />			
			<input type="text" name="personInfo" value="${this.personInfo}" disabled></input>
			<br />			
			<img src="${this.photo.src}" height="200" width="200" alt="${this.photo.alt}">
		</div>
	`;
}
```

Cada vez que se cambie el valor de yearsInCompany, lo recogeremos en un evento y 
	actualizaremos el valor de la propiedad en la función manejadora.

ficha-persona.js

```javascript
render() {
	return html`
		<div>
			<label for="fname">Nombre Completo</label>
			<input type="text" id="fname" name="fname" value="${this.name}" @input="${this.updateName}"></input>
			<br />						
			<label for="yearsInCompany">Años en la empresa</label>
			<input type="text" name="yearsInCompany" value="${this.yearsInCompany}" @input="${this.updateyearsInCompany}"></input>
			<br />			
			<input type="text" name="personInfo" value="${this.personInfo}" disabled></input>
			<br />			
			<img src="${this.photo.src}" height="200" width="200" alt="${this.photo.alt}">
		</div>
	`;
}
```

Actualizamos el valor de la propiedad en la función manejadora.

ficha-persona.js

```javascript
updateyearsInCompany(e) {
	console.log("updateyearsInCompany");
	this.yearsInCompany = e.target.value;
}
```

Ahora, cuando detectemos en la función updated que ha variado el valor 
	de la propiedad yearsInCompany, llamamos a la función para actualizar 
	el valor de personInfo.

ficha-persona.js

```javascript
updated(changedProperties) {	   
	if (changedProperties.has("name")) {
		console.log("Propiedad name cambiada valor anterior era " + changedProperties.get("name") + " nuevo es " + this.name);
	}
		
	if (changedProperties.has("yearsInCompany")) {
		console.log("Propiedad yearsInCompany cambiada valor anterior era " + changedProperties.get("yearsInCompany") + " nuevo es " + this.yearsInCompany);
		this.updatePersonInfo();
	}
}
```


El código completo del componente sería.

ficha-persona.js

```javascript
import { LitElement, html } from 'lit-element';

class FichaPersona extends LitElement {
	
	static get properties() {		
		return {			
			name: {type: String},			
			yearsInCompany: {type: Number},			
			personInfo: {type: String},
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
		
		this.updatePersonInfo();
	}
			
	render() {
		return html`
			<div>
				<label for="fname">Nombre Completo</label>
				<input type="text" id="fname" name="fname" value="${this.name}" @input="${this.updateName}"></input>
				<br />						
				<label for="yearsInCompany">Años en la empresa</label>
				<input type="text" name="yearsInCompany" value="${this.yearsInCompany}" @input="${this.updateyearsInCompany}"></input>
				<br />			
				<input type="text" name="personInfo" value="${this.personInfo}" disabled></input>
				<br />			
				<img src="${this.photo.src}" height="200" width="200" alt="${this.photo.alt}">
			</div>
		`;
	}
		
	updated(changedProperties) {	   
		if (changedProperties.has("name")) {
			console.log("Propiedad name cambiada valor anterior era " + changedProperties.get("name") + " nuevo es " + this.name);
		}
		
		if (changedProperties.has("yearsInCompany")) {
			console.log("Propiedad yearsInCompany cambiada valor anterior era " + changedProperties.get("yearsInCompany") + " nuevo es " + this.yearsInCompany);
			this.updatePersonInfo();
		}
	}
	
	updateName(e) {
		console.log("updateName");
		this.name = e.target.value;	  
	}
	
	updateyearsInCompany(e) {
		console.log("updateyearsInCompany");
		this.yearsInCompany = e.target.value;
	}
	
	updatePersonInfo() {
		console.log("updatePersonInfo");
		console.log("yearsInCompany vale " + this.yearsInCompany);
	
		if (this.yearsInCompany >= 7) {
			this.personInfo = "lead";
		} else if (this.yearsInCompany >= 5) {
			this.personInfo = "senior";
		} else if (this.yearsInCompany >= 3) {
			this.personInfo = "team";
		} else {
			this.personInfo = "junior";
		}
	}
}

customElements.define('ficha-persona', FichaPersona)
```
