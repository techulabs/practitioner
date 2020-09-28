summary: Eventos en Persona App - Borrar una persona del listado
id: FRONT01
categories: LitElement
tags: front
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# FRONT01 - Eventos en Persona App - Borrar una persona del listado
<!-- ------------------------ -->
## Overview 
Duration: 45

### FRONT01 - Eventos en Persona App - Borrar una persona del listado - 60 min

Vamos a implementar el borrado de una persona del listado.

Empezaremos añadiendo un footer al card de Bootstrap y el botón para borrar.

persona-ficha-listado.js

```javascript
render() {
    return html`
		<!-- Enlace Bootstrap -->
		<div class="card h-100">
			<img src="${this.photo.src}" alt="${this.photo.alt}" height="50" width="50" class="card-img-top">
				<div class="card-body">
					<h5 class="card-title">${this.name}</h5>
					<p class="card-text">${this.profile}</p>
					<ul class="list-group list-group-flush">
						<li class="list-group-item">${this.yearsInCompany} años en la empresa</li>
					</ul>
				</div>
			<div class="card-footer">
				<button class="btn btn-danger col-5"><strong>X</strong></button>
			</div>				
		</div>
	`;
  }    
```


· El objetivo es "borrar" una persona lo que, a nivel lógico, significa 
	que ya no estará en el array que tenemos en el componente 
	principal, persona-main y no se pintará en el listado.
· Para borrar la persona en concreto necesitamos un identificador, 
	que será el nombre de la persona en nuestro caso.
· El botón borrar lo tenemos en la ficha de la propia persona, así que lo 
	que tenemos que hacer es, de alguna manera, enviar el nombre 
	de la persona en la que se pulsa el botón borrar, que es un 
	persona-ficha-listado, al componente persona-main.
· Para ello usaremos eventos nativos y custom.

Los componentes se comunican con el exterior lanzando eventos, que pueden  
	ser recogidos por otros componentes y un componente puede ser 
	modificado desde el exterior cambiando el valor de propiedades.
	
Entonces, desde el componente persona-ficha-listado, lanzaremos un evento 
	con la información necesaria para que persona-main, que recogerá el evento,
	pueda modificar el listado de personas.
	
Una vez se haya modificado el listado, se volverá a pintar 
	automáticamente y veremos el resultado en pantalla.

Empezaremos recogiendo el click del usuario en el botón borrar de una persona 
	en concreto.
	
Este evento es **click** y es un evento **nativo**.

persona-ficha-listado.js

```javascript
render() {
		return html`
			<!-- Enlace Bootstrap -->
			<div class="card h-100">
				<img src="${this.photo.src}" alt="${this.photo.alt}" height="50" width="50" class="card-img-top">
				<div class="card-body">
					<h5 class="card-title">${this.name}</h5>
					<p class="card-text">${this.profile}</p>
					<ul class="list-group list-group-flush">
						<li class="list-group-item">${this.yearsInCompany} años en la empresa</li>
					</ul>
				</div>
				<div class="card-footer">
					<button @click="${this.deletePerson}" class="btn btn-danger col-5"><strong>X</strong></button>
				</div>				
			</div>
		`;
	}    
```

Y añadimos la función manejadora del evento, que lanzará un evento 
	**custom**, al que llamaremos **delete-person** con la información 
	necesaria para que la persona pueda ser borrada.

persona-ficha-listado.js

```javascript
deletePerson(e) {	
	console.log("deletePerson en persona-ficha-listado");
	console.log("Se va a borrar la persona de nombre " + this.name); 
	
	this.dispatchEvent(
		new CustomEvent("delete-person", {
				detail: {
					name: this.name
				}
			}
		)
	);
}
```

Ahora, en el componente principal, tendremos que recoger el evento
	y asignarle también una función manejadora.

persona-main.js

```javascript
render() {
    return html`
		<!-- Enlace Bootstrap -->
		<h2 class="text-center">Personas</h2>
		<div class="row" id="peopleList">			
			<div class="row row-cols-1 row-cols-sm-4">
				${this.people.map(
					person => 
					html`<persona-ficha-listado 
							name="${person.name}" 
							yearsInCompany="${person.yearsInCompany}" 
							profile="${person.profile}" 
							.photo="${person.photo}"
							@delete-person="${this.deletePerson}">
						</persona-ficha-listado>`
				)}
			</div>
		</div>					
	`;
}    
```

Será esta función manejadora la que realice el borrado.

persona-main.js

```javascript
deletePerson(e) {
	console.log("deletePerson en persona-main");
	console.log("Se va a borrar la persona de nombre " + e.detail.name);
	  
	this.people = this.people.filter(
		person => person.name != e.detail.name
	);
}
```

El código de los componentes quedaría:

persona-ficha-listado.js

```javascript
import { LitElement, html } from 'lit-element';

class PersonaFichaListado extends LitElement {
	
	static get properties() {
		return {
			name: {type: String},
			yearsInCompany: {type: Number},
			profile: {type: String},
			photo: {type: Object}
		};
	}
	
	constructor() {
		super();			
	}
	
	
	render() {
		return html`
			<!-- Enlace Boostrap -->
			<div class="card h-100">
				<img src="${this.photo.src}" alt="${this.photo.alt}" height="50" width="50" class="card-img-top">
				<div class="card-body">
					<h5 class="card-title">${this.name}</h5>
					<p class="card-text">${this.profile}</p>
					<ul class="list-group list-group-flush">
						<li class="list-group-item">${this.yearsInCompany} años en la empresa</li>
					</ul>
				</div>
				<div class="card-footer">
					<button @click="${this.deletePerson}" class="btn btn-danger col-5"><strong>X</strong></button>
				</div>				
			</div>
		`;
	}
	
	deletePerson(e) {	
		console.log("deletePerson en persona-ficha-listado");
		console.log("Se va a borrar la persona de nombre " + this.name); 
	
		this.dispatchEvent(
			new CustomEvent("delete-person", {
					detail: {
						name: this.name
					}
				}
			)
		);
	}
}

customElements.define('persona-ficha-listado', PersonaFichaListado)
```

persona-main.js

```javascript
import { LitElement, html } from 'lit-element';
import '../persona-ficha-listado/persona-ficha-listado.js';

class PersonaMain extends LitElement {		
	
	static get properties() {
		return {			
			people: {type: Array}
		};
	}			
	
	constructor() {
		super();
				
		this.people = [
			{
				name: "Ellen Ripley",
				yearsInCompany: 10,
				profile: "Lorem ipsum dolor sit amet.",
				photo: {
					"src": "./img/persona.jpg",
					"alt": "Ellen Ripley"
				},
				canTeach: false				
			}, {
				name: "Bruce Banner",		
				yearsInCompany: 2,
				profile: "Lorem ipsum.",
				photo: {
					"src": "./img/persona.jpg",
					"alt": "Bruce Banner"
				},
				canTeach: true
			}, {
				name: "Éowyn",
				yearsInCompany: 5,
				profile: "Lorem ipsum dolor sit amet, consectetur adipisici elit, sed eiusmod tempor incidunt ut labore et dolore magna aliqua. Ut enim ad minim veniam.",
				photo: {
					"src": "./img/persona.jpg",
					"alt": "Éowyn"
				},
				canTeach: true
			}, {
				name: "Turanga Leela",
				yearsInCompany: 9,
				profile: "Lorem ipsum dolor sit amet, consectetur adipisici elit, sed eiusmod.",
				photo: {
					"src": "./img/persona.jpg",
					"alt": "Turanga Leela"
				},
				canTeach: true
			}, {
				name: "Tyrion Lannister",
				yearsInCompany: 1,
				profile: "Lorem ipsum.",
				photo: {
					"src": "./img/persona.jpg",
					"alt": "Tyrion Lannister"
				},
				canTeach: false
			}
		];							
	}
			
	render() {
		return html`
			<!-- Enlace Boostrap -->
			<h2 class="text-center">Personas</h2>
			<div class="row" id="peopleList">		
				<div class="row row-cols-1 row-cols-sm-4">
					${this.people.map(
						person => 
						html`<persona-ficha-listado 
								name="${person.name}" 
								yearsInCompany="${person.yearsInCompany}" 
								profile="${person.profile}" 
								.photo="${person.photo}"
								@delete-person="${this.deletePerson}">
							</persona-ficha-listado>`
					)}
				</div>
			</div>					
		`;
	}
	
	deletePerson(e) {
		console.log("deletePerson en persona-main");
		console.log("Se va a borrar la persona de nombre " + e.detail.name);
	  
		this.people = this.people.filter(
			person => person.name != e.detail.name
		);
	}
}

customElements.define('persona-main', PersonaMain)
```

