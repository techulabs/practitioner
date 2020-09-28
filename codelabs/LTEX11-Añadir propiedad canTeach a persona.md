### LTEX11 - Añadir propiedad canTeach a persona - 30 min

El objetivo del ejercicio es añadir y mostrar una propiedad que nos indique 
	si una persona está disponible para dar clase o no. La llamaremos 
	canTeach y será de tipo Boolean. 

Empezaremos añadiéndola al array de personas.

persona-main.js

```javascript
constructor() {
	super();
		
	this.people = [
		{
			"name": "Ellen Ripley",
			"yearsInCompany": 10,
			photo: {
				"src": "./img/persona.jpg",
				"alt": "Ellen Ripley"
			},
			canTeach: false
		}, {
			"name": "Bruce Banner",		
			"yearsInCompany": 2,
			photo: {
				"src": "./img/persona.jpg",
				"alt": "Bruce Banner"
			},
			canTeach: true
		}, {
			"name": "Éowyn",
			"yearsInCompany": 5,
			photo: {
				"src": "./img/persona.jpg",
				"alt": "Éowyn"
			},
			canTeach: true
		}
	];							
}
```

Luego añadimos la propiedad a la ficha del listado.

persona-ficha-listado.js

```javascript
static get properties() {
	return {
		name: {type: String},
		yearsInCompany: {type: Number},
		photo: {type: Object},
		canTeach: {type: Boolean}
	};
}
```

Y la pintamos en la plantilla con un condicional, 
	para que solo se muestre cuando sea true.

persona-ficha-listado.js

```javascript
render() {
    return html`
		<div>
			<label for="fname">Nombre</label>			
			<input type="text" id="fname" name="fname" value="${this.name}"/>
			<br />
			<label for="yearsInCompany">Años en la empresa</label>
			<input type="text" name="yearsInCompany" value="${this.yearsInCompany}"></input>
			<br />
			<img src="${this.photo.src}" height="200" width="200" alt="${this.photo.alt}">
			<br />
			${this.canTeach === true ? html`<strong>Puede dar clase</strong>` : ""}
		</div>
	`;
  }    
```

Nos falta añadirlo a la plantilla principal para pasarlo a la ficha del listado.
	Tendremos que enviarlo como propiedad o el comportamiento no será el esperado.

persona-main.js

```javascript
render() {	
	return html`	
		<h2>Main</h2>
		<main>			
		${this.people.map(
			person => 
				html`<persona-ficha-listado 
						name="${person.name}" 
						yearsInCompany="${person.yearsInCompany}" 
						.photo="${person.photo}"
						.canTeach="${person.canTeach}" >
				</persona-ficha-listado>`
		)}
		</main>
	`;
}    
```


Los componentes main y listado quedarían.

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
				"name": "Ellen Ripley",
				"yearsInCompany": 10,
				photo: {
					"src": "./img/persona.jpg",
					"alt": "Ellen Ripley"
				},
				canTeach: false				
			}, {
				"name": "Bruce Banner",		
				"yearsInCompany": 2,
				photo: {
					"src": "./img/persona.jpg",
					"alt": "Bruce Banner"
				},
				canTeach: true
			}, {
				"name": "Éowyn",
				"yearsInCompany": 5,
				photo: {
					"src": "./img/persona.jpg",
					"alt": "Éowyn"
				},
				canTeach: true
			}
		];							
	}
	
	render() {	
		return html`
			<h2>Main</h2>
			<main>			
			${this.people.map(
				person => 
					html`<persona-ficha-listado 
							name="${person.name}" 
							yearsInCompany="${person.yearsInCompany}" 
							.photo="${person.photo}"
							.canTeach="${person.canTeach}" >
					</persona-ficha-listado>`
			)}
			</main>
		`;
	}    
}

customElements.define('persona-main', PersonaMain)
```

persona-ficha-listado.js

```javascript
import { LitElement, html } from 'lit-element';

class PersonaFichaListado extends LitElement {
	
	static get properties() {
		return {
			name: {type: String},
			yearsInCompany: {type: Number},
			photo: {type: Object},
			canTeach: {type: Boolean}
		};
	}
	
	constructor() {
		super();			
	}
	
	render() {
		return html`
			<div>
				<label for="fname">Nombre</label>			
				<input type="text" id="fname" name="fname" value="${this.name}"/>
				<br />
				<label for="yearsInCompany">Años en la empresa</label>
				<input type="text" name="yearsInCompany" value="${this.yearsInCompany}"></input>
				<br />
				<img src="${this.photo.src}" height="200" width="200" alt="${this.photo.alt}">
				<br />
				${this.canTeach === true ? html`<strong>Puede dar clase</strong>` : ""}
			</div>
		`;
	} 
}

customElements.define('persona-ficha-listado', PersonaFichaListado)
```
