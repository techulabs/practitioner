### LTEX20 - Eventos en Persona App - ESTADÍSTICAS: CONTADOR DE ELEMENTOS - 60 min

Vamos a añadir un contador que indique cuantas personas tenemos en
el listado, que situaremos en la barra lateral.

Para conseguirlo necesitamos que el número de personas que hay en el array 
llegue a la barra lateral, pero el array está en el componente persona-main.

Vamos a:

· Cada vez que se actualice el array de personas, lanzar un evento.
· Este evento será recogido por el componente principal, persona-app.
· Persona-app "sabe" que hay alguna otra parte de nuestra aplicación que
	necesita el valor del array actualizado para extraer información.
· En lugar de hacer que el propio persona-app procese el array, o enviar 
	ese array directamente a persona-sidebar, lo que haremos será crear 
	un componente nuevo, persona-stats, que recibirá el valor actualizado 
	del array y de ahí extraerá información.
· Este componente tendrá una particularidad además: no tendrá plantilla. 
	Se dedicará solo a procesar el array de personas.
· De esta forma además conseguimos liberar de responsabilidades a persona-sidebar.
· Cuando persona-stats tenga valores nuevos que presentar, lanzará un evento, 
	que será recogido por persona-app y que enviará la información a 
	persona-sidebar, que solo tendrá que mostrarla en su plantilla.

Empezaremos lanzando un evento cada vez que se actualicen las personas.

persona-main.js

```javascript
updated(changedProperties) {
	console.log("updated");	
	if (changedProperties.has("showPersonForm")) {
		console.log("Ha cambiado el valor de la propiedad showPersonForm en persona-main");
		if (this.showPersonForm === true) {
			this.showPersonFormData();
		} else {
			this.showPersonList();
		}
	}
		
	if (changedProperties.has("people")) {
		console.log("Ha cambiado el valor de la propiedad people en persona-main");
		this.dispatchEvent(new CustomEvent("updated-people", {
			detail: {
				people: this.people
			}
			})
		)
	}
}
```

Si en este punto hacemos pruebas para ver que se está lanzando el evento, 
nos encontraremos que:

· En el caso del borrado de una persona el evento se lanza.
· Pero en los casos de actualización de una persona y añadir una persona nueva, 
	el evento no se lanza.
	
Que el evento no se lance significa que la propiedad "people", esto es, 
el array de personas en persona-main, no se está actualizando, pero 
hemos podido comprobar hasta ahora que sí es así. 

El motivo tiene que ver con cómo LitElement evalua un array como 
	candidato a considerar la propiedad como cambiada. 
	
En el caso de la actualización, podemos usar la función map que, 
	de forma similar a filter, devuelve un array nuevo.

Para cuando vayamos a añadir una persona nueva, podemos usar la "spread syntax" 
de Javascript, de forma que directamente igualaremos el array de personas al 
array actual más un elemento al final.

persona-main.js

```javascript
personFormStore(e) {
	console.log("personFormStore");	
	console.log(e.detail.person);		
	  			  		
	if (e.detail.editingPerson === true) {
		console.log("Se va a actualizar la persona de nombre " + e.detail.person.name);
		
		this.people = this.people.map(
			person => person.name === e.detail.person.name 
				? person = e.detail.person : person);
	} else {		  
		console.log("Se va a almacenar una persona nueva");
		this.people = [...this.people, e.detail.person];
	}	  	  	  			  				
		
	console.log("Proceso terminado.");
	console.log(this.people);
  		
	this.showPersonForm = false;
}
```

A continuación crearemos el componente persona-stats.js de la 
	forma habitual. Le pondremos una plantilla de prueba.
	
persona-stats.js

```javascript
import { LitElement, html } from 'lit-element';

class PersonaStats extends LitElement {
	
	static get properties() {
		return {			
		};
	}
	
	constructor() {
		super();			
	}
	
	render() {
		return html`	
			<h1>Prueba Persona Stats</h1>
		`;
	}    
}

customElements.define('persona-stats', PersonaStats)
```

Lo incluiremos en persona-app para probar que funciona primero.

persona-app.js

```javascript
import { LitElement, html } from 'lit-element';
import '../persona-header/persona-header.js';
import '../persona-main/persona-main.js';
import '../persona-footer/persona-footer.js';
import '../persona-sidebar/persona-sidebar.js';
import '../persona-stats/persona-stats.js';

// Other component code.

	render() {
		return html`
		<!-- Enlace Bootstrap -->
		<persona-header></persona-header>
		<div class="row">
			<persona-sidebar class="col-2" @new-person="${this.newPerson}"></persona-sidebar>
			<persona-main class="col-10"></persona-main>
		</div>			
		<persona-footer></persona-footer>
		<persona-stats></persona-stats>
		`;
	}    
	
// Other component code.
```

Una vez comprobamos que funciona quitamos la función render y 
	añadimos una propiedad people, que inicializamos en el constructor.  

persona-stats.js

```javascript
import { LitElement, html } from 'lit-element';

class PersonaStats extends LitElement {
	
	static get properties() {
		return {			
			people: {type: Array}
		};
	}
	
	constructor() {
		super();
		
		this.people = [];
	}	
}

customElements.define('persona-stats', PersonaStats)
```

Cuando se actualice el valor de esta propiedad se llamará a una función 
	para sacar información del array de personas.
	
persona-stats.js

```javascript
updated(changedProperties) {	
	console.log("updated en persona-stats");
	console.log(changedProperties);		
		
	if (changedProperties.has("people")) {
		console.log("Ha cambiado el valor de la propiedad people en persona-stats");
			
		let peopleStats = this.gatherPeopleArrayInfo(this.people);
		this.dispatchEvent(new CustomEvent("updated-people-stats", {
			detail: {
				peopleStats: peopleStats
			}
		}))
	}
}
```

Del array de personas se sacará su longitud y se enviará en el evento.

persona-stats.js

```javascript
gatherPeopleArrayInfo(people) {
	console.log("gatherPeopleArrayInfo");
		
	let peopleStats = {};
	peopleStats.numberOfPeople = people.length;
		
	return peopleStats;
}
```

Preparamos el componente persona-sidebar para recibir los datos. Añadimos la 
propiedad peopleStats y la inicializamos. 

persona-sidebar.js

```javascript
import { LitElement, html } from 'lit-element';

class PersonaSidebar extends LitElement {		
	
	static get properties() {
		return {
			peopleStats: {type: Object}
		};
	}
	
	constructor() {
		super();

		this.peopleStats = {};
	}
	
	// ...
}

customElements.define('persona-sidebar', PersonaSidebar)
```

Y la pintamos en la plantilla.

persona-sidebar.js

```javascript		
render() {
	return html`		
		<!-- Enlace Bootstrap -->
		<aside>
			<section>				
				<div>
					Hay <span class="badge badge-pill badge-primary">${this.peopleStats.numberOfPeople}</span> personas
				<div>			
				<div class="mt-5">
					<button class="w-100 btn bg-success" style="font-size: 50px" @click="${this.newPerson}"><strong>+</strong></button>
				<div>				
			</section>
		</aside>
	`;
}    
```

Ahora pasamos a persona-sidebar el valor del número de personas desde 
persona-app cuando llegue el evento lanzado desde persona-stats.

persona-app.js

```javascript
peopleStatsUpdated(e) {
	console.log("peopleStatsUpdated");
	console.log(e.detail);
	this.shadowRoot.querySelector("persona-sidebar").peopleStats = e.detail.peopleStats;
}
```

Necesitamos preparar también persona-app para recibir el array de personas 
	actualizado desde persona-main. Añadimos la propiedad people.

persona-app.js

```javascript	
static get properties() {
	return {			
		people: {type: Array}
	};
}
```

Capturamos el evento de actualización de las personas que viene de persona-main.

persona-app.js

```javascript	
render() {
	return html`
		<!-- Enlace Bootstrap -->
		<persona-header></persona-header>
		<div class="row">
			<persona-sidebar class="col-2" @new-person="${this.newPerson}"></persona-sidebar>
			<persona-main @updated-people="${this.updatePeople}" class="col-10"></persona-main>
		</div>			
		<persona-footer></persona-footer>
		<persona-stats @updated-people-stats="${this.peopleStatsUpdated}"></persona-stats>
	`;
}    
```

Añadimos la función manejadora, que actualizará el valor 
	de la propiedad people.

persona-app.js

```javascript
updatePeople(e) {
	console.log("updatePeople");
	this.people = e.detail.people;		
}
```

Y la función updated para que cuando haya cambios en el valor 
	del array de personas, lo enviemos a persona-stats para que 
	se vuelvan a calcular las estadísticas.

persona-app.js

```javascript
updated(changedProperties) {
	console.log("updated en persona-app");
	console.log(changedProperties);		
		
	if (changedProperties.has("people")) {		
		console.log("Ha cambiado el valor de la propiedad people en persona-app");
		this.shadowRoot.querySelector("persona-stats").people = this.people;
	}
}
```

Los componentes quedarían tal que.

persona-app.js

```javascript
import { LitElement, html } from 'lit-element';
import '../persona-header/persona-header.js';
import '../persona-main/persona-main.js';
import '../persona-footer/persona-footer.js';
import '../persona-sidebar/persona-sidebar.js';
import '../persona-stats/persona-stats.js';

class PersonaApp extends LitElement {
	
	static get properties() {
		return {			
			people: {type: Array}
		};
	}
	
	constructor() {
		super();			
	}
	
	render() {
		return html`
			<!-- Enlace Bootstrap -->
			<persona-header></persona-header>
			<div class="row">
				<persona-sidebar class="col-2" @new-person="${this.newPerson}"></persona-sidebar>
				<persona-main @updated-people="${this.updatePeople}" class="col-10"></persona-main>
			</div>			
			<persona-footer></persona-footer>
			<persona-stats @updated-people-stats="${this.peopleStatsUpdated}"></persona-stats>
		`;
	}    
	
	newPerson(e) {
		console.log("newPerson en PersonaApp");	
		this.shadowRoot.querySelector("persona-main").showPersonForm = true; 	  	
	}
	
	peopleStatsUpdated(e) {
		console.log("peopleStatsUpdated");
		console.log(e.detail);
		this.shadowRoot.querySelector("persona-sidebar").peopleStats = e.detail.peopleStats;
	}
	
	updatePeople(e) {
		console.log("updatePeople");
		this.people = e.detail.people;
	}
	
	updated(changedProperties) {
		console.log("updated en persona-app");
		console.log(changedProperties);
		
		if (changedProperties.has("people")) {
			console.log("Ha cambiado el valor de la propiedad people en persona-app");
			this.shadowRoot.querySelector("persona-stats").people = this.people;
		}
	}
}

customElements.define('persona-app', PersonaApp)
```

persona-main.js

```javascript
import { LitElement, html } from 'lit-element';
import '../persona-ficha-listado/persona-ficha-listado.js';
import '../persona-form/persona-form.js';

class PersonaMain extends LitElement {		
	
	static get properties() {
		return {			
			people: {type: Array},
			showPersonForm: {type: Boolean}
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

		this.showPersonForm = false;
	}
			
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
								@delete-person="${this.deletePerson}"
								@info-person="${this.infoPerson}">
							</persona-ficha-listado>`
					)}
				</div>
			</div>
			<div class="row">
				<persona-form id="personForm" class="d-none border rounded border-primary"
					@persona-form-close="${this.personFormClose}"
					@persona-form-store="${this.personFormStore}" >
				</persona-form>
			</div>
		`;
	}
	
	updated(changedProperties) {
		console.log("updated");	
		if (changedProperties.has("showPersonForm")) {
			console.log("Ha cambiado el valor de la propiedad showPersonForm en persona-main");
			if (this.showPersonForm === true) {
				this.showPersonFormData();
			} else {
				this.showPersonList();
			}
		}
		
		if (changedProperties.has("people")) {
			console.log("Ha cambiado el valor de la propiedad people en persona-main");			
			this.dispatchEvent(new CustomEvent("updated-people", {
				detail: {
					people: this.people
				}
				})
			)
		}
	}
	
	deletePerson(e) {
		console.log("deletePerson en persona-main");
		console.log("Se va a borrar la persona de nombre " + e.detail.name);
	  
		this.people = this.people.filter(
			person => person.name != e.detail.name
		);
	}
	
	showPersonList() {
		console.log("showPersonList");
		console.log("Mostrando listado de personas");
		this.shadowRoot.getElementById("peopleList").classList.remove("d-none");
		this.shadowRoot.getElementById("personForm").classList.add("d-none");	  
	}
    
	showPersonFormData() {
		console.log("showPersonFormData");
		console.log("Mostrando formulario de persona");
		this.shadowRoot.getElementById("personForm").classList.remove("d-none");	  
		this.shadowRoot.getElementById("peopleList").classList.add("d-none");	 	  
	}
	
	personFormClose() {
		console.log("personFormClose");
		console.log("Se ha cerrado el formulario de la persona");
	  
		this.showPersonForm = false;	
	}
	
	personFormStore(e) {
		console.log("personFormStore");	
		console.log(e.detail.person);		
	  			  		
		if (e.detail.editingPerson === true) {
			console.log("Se va a actualizar la persona de nombre " + e.detail.person.name);
		
			this.people = this.people.map(
				person => person.name === e.detail.person.name 
					? person = e.detail.person : person);
		} else {		  
			console.log("Se va a almacenar una persona nueva");
			this.people = [...this.people, e.detail.person];
		}	  	  	  			  				
		
		console.log("Proceso terminado.");
		console.log(this.people);
  		
		this.showPersonForm = false;
	}
	
	infoPerson(e) {	  
		console.log("infoPerson en persona-main");
		console.log("Se ha pedido más información de la persona " + e.detail.name);	  	  	  
	
		let chosenPerson = this.people.filter(
			person => person.name === e.detail.name
		)
	  
		this.shadowRoot.getElementById("personForm").person = chosenPerson[0];	  	  
		this.shadowRoot.getElementById("personForm").editingPerson = true;
		this.showPersonForm = true;  	  
	}
}

customElements.define('persona-main', PersonaMain)
```

persona-sidebar.js

```javascript
import { LitElement, html } from 'lit-element';

class PersonaSidebar extends LitElement {
	
	static get properties() {
		return {
			peopleStats: {type: Object}
		};
	}
	
	constructor() {
		super();

		this.peopleStats = {};
	}
	
	render() {
		return html`		
			<!-- Enlace Bootstrap -->
			<aside>
				<section>				
					<div>
						Hay <span class="badge badge-pill badge-primary">${this.peopleStats.numberOfPeople}</span> personas
					<div>			
					<div class="mt-5">
						<button class="w-100 btn bg-success" style="font-size: 50px" @click="${this.newPerson}"><strong>+</strong></button>
					<div>				
				</section>
			</aside>
		`;
	}    
	
	newPerson(e) {
		console.log("newPerson en persona-sidebar");
		console.log("Se va a crear una nueva persona");
	  
		this.dispatchEvent(new CustomEvent("new-person", {})); 
	}
}

customElements.define('persona-sidebar', PersonaSidebar)
```

persona-stats.js

```javascript
import { LitElement, html } from 'lit-element';

class PersonaStats extends LitElement {

static get properties() {
	return {			
		people: {type: Array}
	};
}

constructor() {
	super();
	
	this.people = [];
}	

updated(changedProperties) {	
	console.log("updated en persona-stats");
	console.log(changedProperties);		
		
		if (changedProperties.has("people")) {
			console.log("Ha cambiado el valor de la propiedad people en persona-stats");
			
			let peopleStats = this.gatherPeopleArrayInfo(this.people);			
			this.dispatchEvent(new CustomEvent("updated-people-stats", {
				detail: {
					peopleStats: peopleStats
				}
			}))
		}
	}
	
	gatherPeopleArrayInfo(people) {
		console.log("gatherPeopleArrayInfo");
		
		let peopleStats = {};
		peopleStats.numberOfPeople = people.length;
		
		return peopleStats;
	}
}

customElements.define('persona-stats', PersonaStats)
```
