### LTEX18 - Eventos en Persona App - Añadir una persona al listado - 90 min

Vamos a implementar la adición de una nueva persona al listado. Para 
conseguirlo, dividiremos el proceso en varias partes.

El primer paso será añadir una barra lateral a nuestro diseño, en la 
que pondremos el botón de añadir la persona.

Empezamos creando el nuevo componente persona-sidebar de la forma habitual.

persona-sidebar.js

```javascript
import { LitElement, html } from 'lit-element';

class PersonaSidebar extends LitElement {
	
	static get properties() {
		return {			
		};
	}
	
	constructor() {
		super();			
	}
	
	render() {
		return html`		
			<!-- Enlace Bootstrap -->
			<aside>
				<section>				
					<div class="mt-5">
						<button class="w-100 btn bg-success" style="font-size: 50px"><strong>+</strong></button>
					<div>				
				</section>
			</aside>
		`;
	}    
}

customElements.define('persona-sidebar', PersonaSidebar)
```

Posicionamos la barra en el lateral izquierdo, modificamos la estructura 
de toda la app en el componente principal.

persona-app.js

```javascript
import { LitElement, html } from 'lit-element';
import '../persona-header/persona-header.js';
import '../persona-main/persona-main.js';
import '../persona-footer/persona-footer.js';
import '../persona-sidebar/persona-sidebar.js';

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
			<!-- Enlace Bootstrap -->
			<persona-header></persona-header>
			<div class="row">
				<persona-sidebar class="col-2"></persona-sidebar>
				<persona-main class="col-10"></persona-main>
			</div>			
			<persona-footer></persona-footer>
		`;
	}    
}

customElements.define('persona-app', PersonaApp)
```

Para almacenar una nueva persona, necesitamos primero un formulario en el 
que introducir los datos.

Creamos este nuevo componente: persona-form.

persona-form.js

```javascript
import { LitElement, html } from 'lit-element';

class PersonaForm extends LitElement {
	
	static get properties() {
		return {			
		};
	}
	
	constructor() {
		super();				
	}
	
	render() {
		return html`	
			<!-- Enlace Bootstrap -->		
			<div>
				<form>
					<div class="form-group">
						<label>Nombre Completo</label>
						<input type="text" id="personFormName" class="form-control" placeholder="Nombre Completo"/>
					<div>
					<div class="form-group">
						<label>Perfil</label>
						<textarea class="form-control" placeholder="Perfil" rows="5"></textarea>
					<div>
					<div class="form-group">
						<label>Años en la empresa</label>
						<input type="text" class="form-control" placeholder="Años en la empresa"/>
					<div>
					<button class="btn btn-primary"><strong>Atrás</strong></button>
					<button class="btn btn-success"><strong>Guardar</strong></button>
				</form>
			</div>
		`;
	}       
}

customElements.define('persona-form', PersonaForm)
```

Lo importamos y añadimos al componente principal y comprobamos que funciona.

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
		<div class="row">
			<persona-form id="personForm">
			</persona-form>
		</div>
	`;
}
```

Ahora vamos a hacer que solo se vea el formulario o el listado de personas 
según la situación:

· Por defecto se mostrará el listado de personas.
· Cuando se pulse el botón de añadir una persona, se mostrará el formulario 
	y se ocultará el listado.
· Cuando en el formulario se pulse el botón de guardar o atrás, se volverá a 
	mostrar el listado.

Empezamos ocultando el formulario de la persona añadiéndole la clase d-none.

Añadimos también clases para ponerle un borde de color.

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
		<div class="row">
			<persona-form id="personForm" class="d-none border rounded border-primary">
			</persona-form>
		</div>
	`;
}
```


En la barra lateral, capturamos el evento click en el botón de crear una persona.

persona-sidebar.js

```javascript
render() {
    return html`	
		<!-- Enlace Bootstrap -->
		<aside>
			<section>				
				<div class="mt-5">
					<button @click="${this.newPerson}" class="w-100 btn bg-success" style="font-size: 50px"><strong>+</strong></button>
				<div>				
			</section>
		</aside>		
	`;
}
```

Y añadimos la función manejadora que lanzará el evento.

persona-sidebar.js

```javascript
newPerson(e) {
	console.log("newPerson en persona-sidebar");
	console.log("Se va a crear una nueva persona");
	  
	this.dispatchEvent(new CustomEvent("new-person", {})); 
}
```

Este evento será recogido en el componente que contiene al sidebar, que es 
el principal de la aplicación: persona-app.

persona-app.js

```javascript
render() {
	return html`
		<!-- Enlace Bootstrap -->
		<persona-header></persona-header>
		<div class="row">
			<persona-sidebar class="col-2" @new-person="${this.newPerson}"></persona-sidebar>
			<persona-main class="col-10"></persona-main>
		</div>			
		<persona-footer></persona-footer>
	`;
}    
```

Ahora, para conseguir el objetivo:

· Añadiremos una propiedad Boolean showPersonForm a persona-main, que por 
	defecto tendrá valor false.
· Usando la función updated, estaremos controlando el valor de showPersonForm:
	cuando sea false, mostraremos el listado de personas; cuando sea true, el 
	formulario.
· El mostrar/ocultar estará gestionado por funciones en persona-main.

Empezamos añadiendo showPersonForm.

persona-main.js

```javascript
static get properties() {
	return {			
		people: {type: Array},
		showPersonForm: {type: Boolean}
	};
}			
```

Y en el constructor la ponemos a false por defecto.

persona-main.js

```javascript
constructor() {
	super();
	
	// People array.
		
	this.showPersonForm = false;
}
```

Añadimos la función updated.

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
}
```

Y las dos funciones asociadas.

persona-main.js

```javascript
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
```

Por último, añadimos la función manejadora del evento click en el 
componente principal, que pondrá en marcha el cambio en persona-main.

persona-app.js

```
newPerson(e) {
	console.log("newPerson en PersonaApp");	
	this.shadowRoot.querySelector("persona-main").showPersonForm = true; 	  	
}
```

Continuamos: añadimos funcionalidad al botón atrás del formulario, 
	capturamos el evento click.

persona-form.js

```
render() {
	return html`	
		<!-- Enlace Bootstrap -->		
		<div>
			<form>
				<div class="form-group">
					<label>Nombre Completo</label>
					<input type="text" id="personFormName" class="form-control" placeholder="Nombre Completo"/>
				<div>
				<div class="form-group">
					<label>Perfil</label>
					<textarea class="form-control" placeholder="Perfil" rows="5"></textarea>
				<div>
				<div class="form-group">
					<label>Años en la empresa</label>
					<input type="text" class="form-control" placeholder="Años en la empresa"/>
				<div>
				<button @click="${this.goBack}" class="btn btn-primary"><strong>Atrás</strong></button>
				<button class="btn btn-success"><strong>Guardar</strong></button>
			</form>
		</div>			
	`;
}       
```

La función manejadora lanzará un evento para informar de que se quiere 
	cerrar el formulario.

persona-form.js

```javascript
goBack(e) {
	console.log("goBack");	  
	e.preventDefault();	
	this.dispatchEvent(new CustomEvent("persona-form-close",{}));	
}
```

El evento persona-form-close será recogido en el componente principal.

persona-main.js

```javascript
<div class="row">
	<persona-form id="personForm" class="d-none border rounded border-primary"
		@persona-form-close="${this.personFormClose}">
	</persona-form>
</div>
```

Y la función manejadora actualizará la propiedad, lo que 
	desencadenará a través de updated que se actualice la plantilla.

persona-main.js

```
personFormClose() {
	console.log("personFormClose");
	console.log("Se ha cerrado el formulario de la persona");
	  
	this.showPersonForm = false;	
}
```

Ahora vamos a continuar con el proceso de guardado de una persona nueva.

La clave estará en capturar el evento click del botón guardar y,
	a partir de ahí, iniciar el proceso de guardado.
	
Además tendremos que recoger los datos del formulario y con ellos, 
crear una persona nueva que se pintará en el listado.

Capturamos el evento click del botón guardar.

persona-form.js

```
render() {
	return html`	
		<!-- Enlace Bootstrap -->		
		<div>
			<form>
				<div class="form-group">
					<label>Nombre Completo</label>
					<input type="text" id="personFormName" class="form-control" placeholder="Nombre Completo"/>
				<div>
				<div class="form-group">
					<label>Perfil</label>
					<textarea class="form-control" placeholder="Perfil" rows="5"></textarea>
				<div>
				<div class="form-group">
					<label>Años en la empresa</label>
					<input type="text" class="form-control" placeholder="Años en la empresa"/>			
				<div>
				<button @click="${this.goBack}" class="btn btn-primary"><strong>Atrás</strong></button>
				<button @click="${this.storePerson}" class="btn btn-success"><strong>Guardar</strong></button>
			</form>
		</div>			
	`;
}       	
```

Ahora vamos a recoger y enviar la información de la persona.

Lo haremos en un objeto y la enviaremos en un evento 
	(asumiremos siempre la misma imagen por defecto).

Empezamos añadiendo la propiedad person en el formulario.

persona-form.js

```javascript
static get properties() {
	return {			
		person: {type: Object}
	};
}
```

Inicializamos el objeto persona en el constructor.

persona-form.js

```javascript
constructor() {
	super();		
	
	this.person = {};		
}
```

Cuando se actualicen los valores de los inputs de texto, 
	actualizaremos el objeto persona con el nuevo valor, para 
	ello hacemos los data binding necesarios.

La plantilla quedaría.

person-form.js

```javascript
render() {
		return html`	
		<!-- Enlace Bootstrap -->		
			<div>
				<form>
					<div class="form-group">
						<label>Nombre Completo</label>
						<input type="text" @input="${this.updateName}" id="personFormName" class="form-control" placeholder="Nombre Completo"/>
					<div>
					<div class="form-group">
						<label>Perfil</label>
						<textarea @input="${this.updateProfile}" class="form-control" placeholder="Perfil" rows="5"></textarea>
					<div>
					<div class="form-group">
						<label>Años en la empresa</label>
						<input type="text" @input="${this.updateYearsInCompany}" class="form-control" placeholder="Años en la empresa"/>
					<div>
					<button @click="${this.goBack}" class="btn btn-primary"><strong>Atrás</strong></button>
					<button @click="${this.storePerson}" class="btn btn-success"><strong>Guardar</strong></button>
				</form>
			</div>			
		`;
	}       	
```

Añadimos las funciones manejadoras que actualizan el valor de las propiedades.

persona-form.js

```javascript
updateName(e) {
	console.log("updateName");
	console.log("Actualizando la propiedad name con el valor " + e.target.value);
	this.person.name = e.target.value;
}
	
updateProfile(e) {
	console.log("updateProfile");
	console.log("Actualizando la propiedad profile con el valor " + e.target.value);
	this.person.profile = e.target.value;
}
	
updateYearsInCompany(e) {
	console.log("updateYearsInCompany");
	console.log("Actualizando la propiedad yearsInCompany con el valor " + e.target.value);
	this.person.yearsInCompany = e.target.value;
}
```

Y la función manejadora del click en el botón guardar, que prepara un 
	objeto para enviar junto con el evento.

persona-form.js

```javascript
storePerson(e) {
	console.log("storePerson");
	e.preventDefault();
		
	this.person.photo = {
		"src": "./img/persona.jpg",
		"alt": "Persona"
	};
			
	console.log("La propiedad name vale " + this.person.name);
	console.log("La propiedad profile vale " + this.person.profile);
	console.log("La propiedad yearsInCompany vale " + this.person.yearsInCompany);	
			
	this.dispatchEvent(new CustomEvent("persona-form-store",{
		detail: {
			person:  {
					name: this.person.name,
					profile: this.person.profile,
					yearsInCompany: this.person.yearsInCompany,
					photo: this.person.photo
				}
			}
		})
	);
}
```

Recogemos el evento de creación de la persona en persona-main.

persona-main.js

```javascript
<!-- Rest of template -->
<div class="row">
	<persona-form id="personForm" class="d-none border rounded border-primary"
		@persona-form-close="${this.personFormClose}"
		@persona-form-store="${this.personFormStore}" >
	</persona-form>
</div>
```

Y la función manejadora del evento, que guardará la nueva persona.

persona-main.js

```javascript
personFormStore(e) {
	console.log("personFormStore");
	console.log("Se va a almacenar una persona");	
	  			  		
	this.people.push(e.detail.person);
	  
	console.log("Persona almacenada");	
   		
	this.showPersonForm = false;
}	
```

Los componentes quedarían:

persona-app.js

```javascript
import { LitElement, html } from 'lit-element';
import '../persona-header/persona-header.js';
import '../persona-main/persona-main.js';
import '../persona-footer/persona-footer.js';
import '../persona-sidebar/persona-sidebar.js';

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
			<!-- Enlace Bootstrap -->		
			<persona-header></persona-header>
			<div class="row">
				<persona-sidebar class="col-2" @new-person="${this.newPerson}"></persona-sidebar>
				<persona-main class="col-10"></persona-main>
			</div>			
			<persona-footer></persona-footer>
		`;
	}    
	
	newPerson(e) {
		console.log("newPerson en PersonaApp");	
		this.shadowRoot.querySelector("persona-main").showPersonForm = true; 	  	
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
								@delete-person="${this.deletePerson}">
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
		console.log("Se va a almacenar una persona");	
	  			  		
		this.people.push(e.detail.person);
	  
		console.log("Persona almacenada");	
   		
		this.showPersonForm = false;
	}	
}

customElements.define('persona-main', PersonaMain)
```

persona-form.js

```javascript
import { LitElement, html } from 'lit-element';

class PersonaForm extends LitElement {
	
	static get properties() {
		return {			
			person: {type: Object}
		};
	}
	
	constructor() {
		super();		
	
		this.person = {};		
	}
	
	render() {
		return html`	
			<!-- Enlace Bootstrap -->		
			<div>
				<form>
					<div class="form-group">
						<label>Nombre Completo</label>
						<input type="text" @input="${this.updateName}" id="personFormName" class="form-control" placeholder="Nombre Completo"/>
					<div>
					<div class="form-group">
						<label>Perfil</label>
						<textarea @input="${this.updateProfile}" class="form-control" placeholder="Perfil" rows="5"></textarea>
					<div>
					<div class="form-group">
						<label>Años en la empresa</label>
						<input type="text" @input="${this.updateYearsInCompany}" class="form-control" placeholder="Años en la empresa"/>
					<div>
					<button @click="${this.goBack}" class="btn btn-primary"><strong>Atrás</strong></button>
					<button @click="${this.storePerson}" class="btn btn-success"><strong>Guardar</strong></button>
				</form>
			</div>			
		`;
	}       	
	
	goBack(e) {
		console.log("goBack");	  
		e.preventDefault();	
		this.dispatchEvent(new CustomEvent("persona-form-close",{}));
	}
	
	updateName(e) {
		console.log("updateName");
		console.log("Actualizando la propiedad name con el valor " + e.target.value);
		this.person.name = e.target.value;
	}
	
	updateProfile(e) {
		console.log("updateProfile");
		console.log("Actualizando la propiedad profile con el valor " + e.target.value);
		this.person.profile = e.target.value;
	}
	
	updateYearsInCompany(e) {
		console.log("updateYearsInCompany");
		console.log("Actualizando la propiedad yearsInCompany con el valor " + e.target.value);
		this.person.yearsInCompany = e.target.value;
	}
	
	storePerson(e) {
		console.log("storePerson");
		e.preventDefault();
		
		this.person.photo = {
			"src": "./img/persona.jpg",
			"alt": "Persona"
		};
			
		console.log("La propiedad name vale " + this.person.name);
		console.log("La propiedad profile vale " + this.person.profile);
		console.log("La propiedad yearsInCompany vale " + this.person.yearsInCompany);
			
		this.dispatchEvent(new CustomEvent("persona-form-store",{
			detail: {
				person:  {
						name: this.person.name,
						profile: this.person.profile,
						yearsInCompany: this.person.yearsInCompany,
						photo: this.person.photo
					}
				}
			})
		);
	}
}

customElements.define('persona-form', PersonaForm)
```

persona-sidebar.js

```javascript
import { LitElement, html } from 'lit-element';

class PersonaSidebar extends LitElement {
	
	static get properties() {
		return {			
		};
	}
	
	constructor() {
		super();			
	}
	
	render() {
		return html`		
			<!-- Enlace Bootstrap -->		
			<aside>
				<section>				
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
