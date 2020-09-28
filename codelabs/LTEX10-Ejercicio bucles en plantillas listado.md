### LTEX10 - Ejercicio bucles en plantillas: listado - 60 min

Vamos a pintar un listado de personas, que tendremos almacenadas
	en la propia aplicación.
	
A cada persona le corresponderá una ficha y pintaremos tantas fichas 
	como personas tengamos almacenadas.

Empezaremos creando el componente persona-ficha-listado, para la ficha.
	Por ahora solo añadiremos la propiedad name.

persona-ficha-listado.js

```javascript
import { LitElement, html } from 'lit-element';

class PersonaFichaListado extends LitElement {
	
	static get properties() {
		return {
			name: {type: String}			
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
			</div>
		`;
	}    
}

customElements.define('persona-ficha-listado', PersonaFichaListado)
```

Añadimos las personas como array en las propiedades del componente principal.

persona-main.js

```javascript
static get properties() {
	return {
		people: {type: Array}
	};
}
```

Usamos un bucle para mostrar el array de personas, pintando una ficha para 
cada una.

persona-main.js

```javascript
render() {	
    return html`	
		<h2>Main</h2>
		<main>			
		${this.people.map(
			person => html`<persona-ficha-listado></persona-ficha-listado>`
		)}
		</main>
	`;
}    
```

Importamos la ficha del listado en el componente main.

persona-main.js

```javascript
import { LitElement, html } from 'lit-element';
import '../persona-ficha-listado/persona-ficha-listado.js';
```

Creamos personas con nombre en el constructor.

persona-main.js

```javascript
	// Persona Main
	constructor() {
		super();
		this.people = [
			{
				"name": "Ellen Ripley"
			}, {
				"name": "Bruce Banner"				
			}, {
				"name": "Éowyn"
			}
		];
	}	    
```

Y pasamos a la ficha su nombre.

persona-main.js

```javascript
render() {	
    return html`	
		<h2>Main</h2>
		<main>			
		${this.people.map(
			person => html`<persona-ficha-listado name="${person.name}"></persona-ficha-listado>`
		)}
		</main>
	`;
}    
```

Ahora añadiremos la propiedad yearsInCompany.

Empezamos añadiendo la propiedad en la ficha del listado.

persona-ficha-listado.js

```javascript				
static get properties() {
	return {
		name: {type: String},
		yearsInCompany: {type: Number}
	}
};	
```

La pintamos en la plantilla de la ficha.

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
		</div>
	`;
  }   
```

Añadimos los valores al array de personas.

persona-main.js

```javascript
	constructor() {
		super();
		this.people = [
			{
				"name": "Ellen Ripley",
				"yearsInCompany": 10			
			}, {
				"name": "Bruce Banner",			
				"yearsInCompany": 2			
			}, {
				"name": "Éowyn",
				"yearsInCompany": 5
			}					
		];						
	}
```

Y pasamos a la ficha del listado el valor.

persona-main.js

```javascript		    
render() {	
    return html`	
		<h2>Main</h2>
		<main>			
		${this.people.map(
			person => html`<persona-ficha-listado name="${person.name}" 
				yearsInCompany="${person.yearsInCompany}"></persona-ficha-listado>`
		)}
		</main>
	`;
}    
```

Vamos a añadir la foto de la persona.

Primero añadimos la imagen como un objeto al array de personas.

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
			}
		}, {
			"name": "Bruce Banner",		
			"yearsInCompany": 2,
			photo: {
				"src": "./img/persona.jpg",
				"alt": "Bruce Banner"
			}
		}, {
			"name": "Éowyn",
			"yearsInCompany": 5,
			photo: {
				"src": "./img/persona.jpg",
				"alt": "Éowyn"
			}
		}
	];										
}
```

A continuación le pasamos al componente listado la imagen.

Al tratarse de un tipo de datos complejo, tendremos 
que pasar el valor como una propiedad, o no funcionará.

persona-main.js

```javascript
render() {	
	return html`	
		<h2>Main</h2>
		<main>			
		${this.people.map(
			person => html`<persona-ficha-listado name="${person.name}" 
				yearsInCompany="${person.yearsInCompany}" .photo="${person.photo}"></persona-ficha-listado>`
		)}
		</main>
	`;
}    
```

Añadimos la propiedad photo a la ficha del listado.

persona-ficha-listado.js

```javascript
static get properties() {
	return {
		name: {type: String},
		yearsInCompany: {type: Number},
		photo: {type: Object}
	};
}
```

Y modificamos la plantilla de listado.

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
		</div>
	`;
}
```

Los componentes main y ficha quedarían:

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
				}
			}, {
				"name": "Bruce Banner",		
				"yearsInCompany": 2,
				photo: {
					"src": "./img/persona.jpg",
					"alt": "Bruce Banner"
				}
			}, {
				"name": "Éowyn",
				"yearsInCompany": 5,
				photo: {
					"src": "./img/persona.jpg",
					"alt": "Éowyn"
				}
			}
		];							
	}
	
	render() {	
		return html`	
			<h2>Main</h2>
			<main>			
			${this.people.map(
				person => html`<persona-ficha-listado name="${person.name}" 
					yearsInCompany="${person.yearsInCompany}" .photo=${person.photo}></persona-ficha-listado>`
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
			photo: {type: Object}
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
			</div>
		`;
	} 
}

customElements.define('persona-ficha-listado', PersonaFichaListado)
```
