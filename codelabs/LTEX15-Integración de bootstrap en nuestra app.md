### LTEX15 - Integración de bootstrap en nuestra app - 60 min

Vamos a utilizar Bootstrap para mejorar el diseño de nuestra aplicación.

Empezamos enlazando Bootstrap en los componentes persona-main 
	y persona-ficha-listado, tal y como hemos aprendido en el ejercicio
	de estilado básico con Bootstrap.
	
Modificamos el título del main y centramos el texto.

persona-main.js

```javascript
	<h2 class="text-center">Personas</h2>
```

En persona-ficha-listado vamos a quitar los atributos height y width 
de la etiqueta img de la persona, que quedará tal que:

persona-ficha-listado.js

```javascript
	<img src="${this.photo.src}" alt="${this.photo.alt}">
```

Añadimos más personas al listado.

persona-main.js

```javascript
[
	// Other people up here.
	{
		name: "Turanga Leela",
		yearsInCompany: 9,
		photo: {
			"src": "./img/persona.jpg",
			"alt": "Turanga Leela"
		},
		canTeach: true
	}, {
		name: "Tyrion Lannister",
		yearsInCompany: 1,				
		photo: {
			"src": "./img/persona.jpg",
			"alt": "Tyrion Lannister"
		},
		canTeach: false
	}
];
```

Ahora vamos a preparar el diseño para que haya cuatro personas por fila.

Lo que haremos será añadir una fila en el componente principal, 
que actuará de contenedor para el listado de personas entero.

Luego añadiremos otra fila, que serán las personas listadas en si. 

Esta fila de personas la haremos de una columna para 
dispositivos más pequeños y de 4 columnas para el resto.

persona-main.js

```
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
							.photo="${person.photo}" >
					</persona-ficha-listado>`
			)}
			</div>
		</div>					
	`;
}
```

A continuación vamos a estilar la ficha de cada persona con más detalle:

· Usaremos el componente **card** de Bootstrap.
· Cada persona tendrá un formato de "tarjeta".
· La imagen de la persona irá en la parte superior.
· Ajustaremos el tamaño de la imagen directamente a un cuadrado de 50x50.
· En el cuerpo de la tarjeta irá el nombre de la persona e información.
· Ponemos la lista como "flush" para que no salga recuadrada.

persona-ficha-listado.js

```javascript
render() {
    return html`
		<!-- Enlace de bootstrap -->
		<div class="card">
			<img src="${this.photo.src}" alt="${this.photo.alt}" height="50" width="50" class="card-img-top">
			<div class="card-body">
				<h5 class="card-title">${this.name}</h5>
				<ul class="list-group list-group-flush">
					<li class="list-group-item">${this.yearsInCompany} años en la empresa</li>
				</ul>
			</div>
		</div>
	`;
  }    
```

Para seguir, añadimos una nueva propiedad **profile** 
para tener información más detallada de la persona.

Empezamos añadiendo la propiedad al array de personas y 
	le damos valor con lorem ipsum variando (resto de propiedades omitidas).

persona-main.js

```javascript
this.people = [
		{				
			profile: "Lorem ipsum dolor sit amet.",
		}, {				
			profile: "Lorem ipsum.",				
		}, {				
			profile: "Lorem ipsum dolor sit amet, consectetur adipisici elit, sed eiusmod tempor incidunt ut labore et dolore magna aliqua. Ut enim ad minim veniam.",
		}, {				
			profile: "Lorem ipsum dolor sit amet, consectetur adipisici elit, sed eiusmod.",
		}, {				
			profile: "Lorem ipsum.",					
		}
	]			
}
```

Y la pasamos como atributo al componente.

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
						.photo="${person.photo}" >
					</persona-ficha-listado>`
			)}
			</div>
		</div>					
	`;
}
```

Añadimos la propiedad en la ficha del listado.

persona-ficha-listado.js

```javascript
static get properties() {
	return {
		name: {type: String},
		yearsInCompany: {type: Number},
		photo: {type: Object},
		profile: {type: String}
	};
}
```

Y a la plantilla del listado.

persona-ficha-listado.js

```javascript
render() {
    return html`
		<!-- Enlace de bootstrap -->
		<div class="card">
			<img src="${this.photo.src}" alt="${this.photo.alt}" height="50" width="50" class="card-img-top">
			<div class="card-body">
				<h5 class="card-title">${this.name}</h5>
				<p class="card-text">${this.profile}</p>
				<ul class="list-group list-group-flush">
					<li class="list-group-item">${this.yearsInCompany} años en la empresa</li>
				</ul>
			</div>
		</div>
	`;
}    
```

Por último añadimos h-100 al contenedor principal de la tarjeta para que 
	queden todas igual de altas.

persona-ficha-listado.js

```javascript
render() {
    return html`
		<!-- Enlace de bootstrap -->
		<div class="card h-100">
			<img src="${this.photo.src}" alt="${this.photo.alt}" height="50" width="50" class="card-img-top">
			<div class="card-body">
				<h5 class="card-title">${this.name}</h5>
				<p class="card-text">${this.profile}</p>
				<ul class="list-group list-group-flush">
					<li class="list-group-item">${this.yearsInCompany} años en la empresa</li>
				</ul>
			</div>
		</div>
	`;
}    
```
