### LTEX19 - Eventos en Persona App - Consultar y modificar información de una persona - 90 min

Vamos a añadir un botón para que se pueda consultar la información 
	de una persona en el formulario.
	
Empezaremos añadiendo el botón al footer de la ficha de la persona, 
asociándole una función manejadora al evento click.	

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
				<button @click="${this.moreInfo}" class="btn btn-info col-5 offset-1"><strong>Info</strong></button>
			</div>				
		</div>
	`;
}
```

Usaremos como identificador de la persona su nombre, que enviaremos 
	en un evento lanzado en la función manejadora asociada 
	al evento click.

persona-ficha-listado.js

```javascript
moreInfo(e) {
	console.log("moreInfo");
	console.log("Se ha pedido más información de la persona " + this.name);	  
	  
	this.dispatchEvent(
		new CustomEvent("info-person", {
			detail: {
				name: this.name,				
			}					 
		})
	); 
}
```

Capturamos el evento en el componente principal. 

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
```

En la función manejadora localizaremos la persona de la que estamos pidiendo 
	información y se la pasaremos directamente al formulario como valor 
	de una propiedad.

persona-main.js

```javascript
infoPerson(e) {	  
	console.log("infoPerson en persona-main");
	console.log("Se ha pedido más información de la persona " + e.detail.name);
	
	let chosenPerson = this.people.filter(
		person => person.name === e.detail.name
	)
	  
	this.shadowRoot.getElementById("personForm").person = chosenPerson[0];	  	  
	this.showPersonForm = true;  	  
}
```

En este punto podremos ver el formulario, pero necesitamos que se muestren 
	los datos en cada campo de texto para lo que los enlazaremos 
	en los inputs.

persona-form.js

```
render() {
	return html`
		<!-- Enlace Bootstrap -->
		<div>
			<form>
				<div class="form-group">
					<label>Nombre Completo</label>
					<input type="text" @input="${this.updateName}" .value="${this.person.name}" id="personFormName" class="form-control" placeholder="Nombre Completo"/>
				<div>
				<div class="form-group">
					<label>Perfil</label>
					<textarea @input="${this.updateProfile}" .value="${this.person.profile}" class="form-control" placeholder="Perfil" rows="5"></textarea>
				<div>
				<div class="form-group">
					<label>Años en la empresa</label>
					<input type="text" @input="${this.updateYearsInCompany}" .value="${this.person.yearsInCompany}" class="form-control" placeholder="Años en la empresa"/>			
				<div>
				<button @click="${this.goBack}" class="btn btn-primary"><strong>Atrás</strong></button>
				<button @click="${this.storePerson}" class="btn btn-success"><strong>Guardar</strong></button>
			</form>
		</div>			
	`;
}
```

Antes de seguir avanzando al botón guardar, si volvemos a probar 
el resto de funcionalidades que ya teníamos implementadas, nos 
encontraremos con que los cambios que hemos hecho pueden dejar la 
app en un estado que no es el que buscamos.

Si después de consultar la información de cualquier persona, le damos al botón 
de añadir una persona, nos encontraremos la información de la última persona 
que hayamos consultado en el formulario. 

Una solución sería añadir en la función manejadora del click en botón atrás, 
goBack, código para inicializar el valor de los elementos del formulario. 

Como este código sería casi el mismo que hay en el constructor, lo podríamos
sacar a una función aparte y llamarla.

persona-form.js

```javascript
resetFormData() {
	console.log("resetFormData");
	this.person = {};
	this.person.name = "";
	this.person.profile = "";
	this.person.yearsInCompany = "";
}
```

Y la llamamos en el constructor.

persona-form.js

```javascript
constructor() {
	super();		
	
	this.resetFormData();
}
```

Y en la función goBack.

persona-form.js

```javascript
goBack(e) {
	console.log("goBack");	  
	e.preventDefault();
	this.resetFormData();
	this.dispatchEvent(new CustomEvent("cheese-form-close",{}));	
}
```

Asumiremos que el identificador único de una persona es su nombre.
	Lo usaremos para localizar la persona que queremos actualizar y, por lo 
	tanto, no se podrá modificar en el formulario.

Para hacer este cambio en el formulario, necesitaremos distinguir 
	el uso que le estemos dando: edición o creación.

Usaremos una propiedad boolean para indicar si el formulario 
	se está usando para edición.

Indicamos al formulario que cuando se pulse el botón de más información 
	de una persona, lo que estamos haciendo será editar una persona.

persona-main.js

```javascript
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
```

Añadimos la propiedad editingPerson al formulario.

pesona-form.js

```javascript
static get properties() {
	return {			
		person: {type: Object},
		editingPerson: {type: Boolean}
	};
}
```

La inicializamos a false por defecto dentro de la función resetFormData, 
para que así el formulario vuelva al estado inicial tras usarlo.

persona-form.js

```javascript
resetFormData() {
	console.log("resetFormData");
	this.person = {};
	this.person.name = "";
	this.person.profile = "";
	this.person.yearsInCompany = "";
		
	this.editingPerson = false;
}
```

Y hacemos que en la plantilla no se pueda editar el nombre si estamos 
	editando la persona.

persona-form.js

```javascript
render() {
	return html`	
		<!-- Enlace Bootstrap -->
		<div>
			<form>
				<div class="form-group">
					<label>Nombre Completo</label>
					<input type="text" @input="${this.updateName}" .value="${this.person.name}" ?disabled="${this.editingPerson}" id="personFormName" class="form-control" placeholder="Nombre Completo"/>
				<div>
				<div class="form-group">
					<label>Perfil</label>
					<textarea @input="${this.updateProfile}" .value="${this.person.profile}" class="form-control" placeholder="Perfil" rows="5"></textarea>
				<div>
				<div class="form-group">
					<label>Años en la empresa</label>
					<input type="text" @input="${this.updateYearsInCompany}" .value="${this.person.yearsInCompany}" class="form-control" placeholder="Años en la empresa"/>
				<div>
				<button @click="${this.goBack}" class="btn btn-primary"><strong>Atrás</strong></button>
				<button @click="${this.storePerson}" class="btn btn-success"><strong>Guardar</strong></button>
			</form>
		</div>			
	`;
}
```

Refactorizamos el código del guardado de la persona para indicar 
	al formulario que no estamos editando una persona. Enviamos 
	la propiedad editingPerson en el evento de guardado para 
	que, a la hora de ir a guardar, el componente receptor sepa 
	si era una edición o no.

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
			
	this.dispatchEvent(new CustomEvent("persona-form-store", {
		detail: {
			person:  {
					name: this.person.name,
					profile: this.person.profile,
					yearsInCompany: this.person.yearsInCompany,
					photo: this.person.photo
				},
				editingPerson: this.editingPerson
			}
		})
	);
}
```

Y usamos el valor de la propiedad en el componente receptor del 
	evento para decidir	si el proceso será de guardado o actualización.

persona-main.js

```javascript
personFormStore(e) {
	console.log("personFormStore");	
	console.log(e.detail.person);		
	  			  		
	if (e.detail.editingPerson === true) {
		console.log("Se va a actualizar la persona de nombre " + e.detail.person.name);
		let indexOfPerson = 
			this.people.findIndex(
				person => person.name === e.detail.person.name
			);
		if (indexOfPerson >= 0) {
			console.log("Persona encontrada");
			this.people[indexOfPerson] = e.detail.person;
		}
	} else {		  
		console.log("Se va a almacenar una persona nueva");
		this.people.push(e.detail.person);
	}	  	  	  			  				
		
	console.log("Proceso terminado.");
	console.log(this.people);
  		
	this.showPersonForm = false;
}
```

Los componentes quedarían.

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
					<button @click="${this.moreInfo}" class="btn btn-info col-5 offset-1"><strong>Info</strong></button>
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
	
	moreInfo(e) {
		console.log("moreInfo");
		console.log("Se ha pedido más información de la persona " + this.name);
	  
		this.dispatchEvent(
			new CustomEvent("info-person", {
				detail: {
					name: this.name,				
				}					 
			})
		); 
	}
}

customElements.define('persona-ficha-listado', PersonaFichaListado)
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
			let indexOfPerson = 
				this.people.findIndex(
					person => person.name === e.detail.person.name
				);
			if (indexOfPerson >= 0) {
				console.log("Persona encontrada");
				this.people[indexOfPerson] = e.detail.person;
			}
		} else {		  
			console.log("Se va a almacenar una persona nueva");
			this.people.push(e.detail.person);
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

persona-form.js

```javascript
import { LitElement, html } from 'lit-element';

class PersonaForm extends LitElement {
	
	static get properties() {
		return {			
			person: {type: Object},
			editingPerson: {type: Boolean}
		};
	}
	
	constructor() {
		super();		
	
		this.resetFormData();
	}
	
	render() {
		return html`	
			<!-- Enlace Bootstrap -->
			<div>
				<form>
					<div class="form-group">
						<label>Nombre Completo</label>
						<input type="text" @input="${this.updateName}" .value="${this.person.name}" ?disabled="${this.editingPerson}" id="personFormName" class="form-control" placeholder="Nombre Completo"/>
					<div>
					<div class="form-group">
						<label>Perfil</label>
						<textarea @input="${this.updateProfile}" .value="${this.person.profile}" class="form-control" placeholder="Perfil" rows="5"></textarea>
					<div>
					<div class="form-group">
						<label>Años en la empresa</label>
						<input type="text" @input="${this.updateYearsInCompany}" .value="${this.person.yearsInCompany}" class="form-control" placeholder="Años en la empresa"/>			
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
		this.resetFormData();
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
					},
					editingPerson: this.editingPerson
				}
			})
		);
	}
	
	resetFormData() {
		console.log("resetFormData");
		this.person = {};
		this.person.name = "";
		this.person.profile = "";
		this.person.yearsInCompany = "";
		
		this.editingPerson = false;
	}
}

customElements.define('persona-form', PersonaForm)
```
