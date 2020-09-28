### LTEX12 - Prueba petición datos a API REST  - 30 min

En este ejercicio nuestro objetivo es obtener datos de una API REST 
y presentarlos en un componente de prueba.

Empezaremos creando el componente test-api de la forma habitual.

test-api.js

```javascript
import {LitElement, html} from 'lit-element';

class TestApi extends LitElement {

    static get properties() {
		return {
		};
	}
	
	constructor() {
        super();
	}

    render() {
        return html `
            <div>Test Api</div>
        `
    }
}

customElements.define("test-api", TestApi);
```

Y lo añadimos al index.html para revisar que funciona (si es el único 
	componente que ponemos en la plantilla será más sencillo de revisar).	
	
La API que usaremos para hacer la prueba es la Star Wars API, alojada en 
	https://swapi.dev/ . Esta API nos permite obtener datos relacionados con 
	el universo de Star Wars. En concreto lo que haremos será obtener las 
	películas que están almacenadas en la API y mostrar su título y director. 

Añadiremos al componente la propiedad movies, que nos servirá para almacenar 
las películas y la inicializamos en el constructor. 

test-api.js

```javascript
import {LitElement, html} from 'lit-element';

class TestApi extends LitElement {

    static get properties() {
		return {
            movies: {type: Array}
		};
	}
	
	constructor() {
        super();

        this.movies = [];
	}
	
	
	// Render    
}

customElements.define("test-api", TestApi);
```

En la documentación de la API veremos que los campos de las películas 
que queremos extraer se llaman "title" y "director". Con esa información 
añadimos un bucle a la plantilla para que, de todas las películas del array,
nos pinte el título y el director, junto con un texto.

test-api.js

```javascript
import {LitElement, html} from 'lit-element';

class TestApi extends LitElement {

    // Rest of component's code.

    render() {
        return html`
            ${this.movies.map(
				movie => 
				    html`<div>La película ${movie.title}, fue dirigida por ${movie.director}</div>`
            )}
        `;
    }
}

customElements.define("test-api", TestApi);
```

Ahora tendremos que hacer la petición para obtener los datos. Usaremos 
AJAX y haremos una petición asíncrona a la URL que la documentación 
nos indica que contiene las películas usando el método GET. 
Usando la propiedad onload asignamos una función manejadora, comprobamos que 
la petición haya devuelto un código 200 y parseamos la respuesta en texto, 
que tendría que venir en formato JSON. El resultado será 
una variable Javascript con la respuesta de la API. Esta 
respuesta contiene un array con las películas, que podemos 
asignar a la propiedad movies del componente y ya tendríamos las películas 
disponibles para trabajar con ellas.

El código que realiza estas operaciones lo pondremos en una función que 
llamaremos en el constructor del componente.

test-api.js

```javascript
import {LitElement, html} from 'lit-element';

class TestApi extends LitElement {

	// Properties
    	
	constructor() {
        super();

        this.movies = [];
        this.getMovieData();
	}

	// Render

    getMovieData() {
        console.log("getMovieData");
        console.log("Obteniendo datos de las películas");

        let xhr = new XMLHttpRequest();

        xhr.onload = () => {
            if (xhr.status === 200) {
                console.log("Petición completada correctamente");               
                
                let APIResponse = JSON.parse(xhr.responseText);
                                
                this.movies = APIResponse.results;
            }
        };                

        xhr.open("GET", "https://swapi.dev/api/films/");
        xhr.send();
    }
}

customElements.define("test-api", TestApi);
```

El componente quedaría:

test-api.js

```javascript
import {LitElement, html} from 'lit-element';

class TestApi extends LitElement {

    static get properties() {
		return {
            movies: {type: Array}
		};
	}
	
	constructor() {
        super();

        this.movies = [];
        this.getMovieData();
	}

    render() {
        return html`
            ${this.movies.map(
				movie => 
				    html`<div>La película ${movie.title}, fue dirigida por ${movie.director}</div>`
            )}
        `;
    }

    getMovieData() {
        console.log("getMovieData");
        console.log("Obteniendo datos de las películas");

        let xhr = new XMLHttpRequest();

        xhr.onload = () => {
            if (xhr.status === 200) {
                console.log("Petición completada correctamente");               
                
                let APIResponse = JSON.parse(xhr.responseText);
                                
                this.movies = APIResponse.results;
            }
        };                

        xhr.open("GET", "https://swapi.dev/api/films/");
        xhr.send();
    }
}

customElements.define("test-api", TestApi);
```
