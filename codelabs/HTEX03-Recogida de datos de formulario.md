summary: Recogida de datos de formulario
id: HTEX03
categories: HTML
tags: front
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# HTEX03 - Recogida de datos de formulario
<!-- ------------------------ -->
## Overview 
Duration: 60

### HTEX03 - Recogida de datos de formulario: 

	uso de localStorage y sessionStorage - 60 min

En este ejercicio vamos a almacenar y recuperar datos introducidos en un 
formulario utilizando para ello LocalStorage y SessionStorage.

Ambas son parte de las APIs de Web Storage, que nos sirven para almacenar 
datos en formato de pares clave/valor en el navegador.

La diferencia fundamental entre ambas es que en SessionStorage los datos 
persisten en lo que dure la sesión en la que se han almacenado, mientras 
que en LocalStorage persisten incluso al cerrar el navegador.

NOTA - Para ver los datos que almacenemos en los ejemplos, en Chrome 
tendremos que pulsar la tecla F12, en el menú que aparece seleccionar 
"Application" y ahí veremos en la barra lateral la sección "Storage", 
que contiene Local Storage y Session Storage.

Partiremos de la página HTML diseñada en el ejercicio HTEX01 y podremos 
usar también la página https://jsfiddle.net/ para resolver el ejercicio.

Empezaremos añadiendo una función Javascript que recogerá el nombre 
que haya en el input formNombre del formulario. Además, almacenará 
tanto en localStorage como en sessionStorage su valor asociado 
a la "key" name. Esta "key" se usará luego para recuperar el valor.

Con "event.preventDefault();" evitaremos que se envíe el formulario.

```javascript
function storeUserData() {
	console.log("storeUserData");
	event.preventDefault();

	var name = document.getElementById("formNombre").value;
	localStorage.setItem("name", name);
	sessionStorage.setItem("name", name);
}
```

Añadimos un botón al formulario para iniciar el proceso de guardado y a su 
evento click asociamos como función manejadora storeUserData.

```html
<button onclick="storeUserData()">Guardar</button>
```

En este punto podremos comprobar en el navegador que se haya almacenado 
correctamente el valor. Además, si abrimos/cerramos el navegador y/o abrimos 
una nueva pestaña, podremos ver la diferencia en la persistencia de localStorage 
y sessionStorage.

A continuación vamos a hacer el proceso inverso: obtendremos desde localStorage 
el nombre almacenado y lo pintaremos en el input apropiado del formulario.

```javascript
function getUserData() {
	console.log("getUserData");
	event.preventDefault();

	var nameInput = document.getElementById("formNombre");
	nameInput.value = localStorage.getItem("name");
}
```

Añadimos un botón para recuperar el nombre asociando la función manejadora 
del evento click a getUserData.

```html
<button onclick="getUserData()">Recuperar</button>
```

También podemos guardar varios elementos del formulario al mismo tiempo.
Para ello, podemos utilizar objetos Javascript y guardarlos en formato JSON.
Así luego podremos hacer el proceso inverso: recuperar los datos almacenados 
en JSON y pasarlos a objetos Javascript, pudiendo asignarlos a los campos 
del formulario apropiados.

Refactorizamos la función storeUserData para que:

· Recoja el nombre y el email del formulario.
· Usando un objeto, agrupe sus valores.
· Convierta el valor del objeto Javascript a JSON y lo almacene.

```javascript
function storeUserData() {

	console.log("storeUserData");
	event.preventDefault();
	
	var name = document.getElementById("formNombre").value;
	var email = document.getElementById("formEmail").value;

	var userData = {
		"name": name,
		"email": email
	};

    localStorage.setItem("userDataJSON", JSON.stringify(userData));
}
```

Refactorizamos también getUserData para que:

· Recupere los datos guardados en JSON.
· Los convierta a una variable Javascript.
· Extraiga los datos y los asigne a los inputs apropiados del formulario.

```javascript
function getUserData() {
	console.log("getUserData");
	event.preventDefault();

	var userData = JSON.parse(localStorage.getItem("userDataJSON"));
	
	document.getElementById("formNombre").value = userData.name;
	document.getElementById("formEmail").value = userData.email;	
}
```

El HTML quedaría tal que:

```html
<!doctype html>
<html>
  <head>
    <title>This is a test</title>
  </head>
  <body>
    <h1>¡Hola Mundo!</h1>
    <form>
		<label>Nombre</label>
        <input id="formNombre" type="text" required placeholder="Nombre" />
        <br />
		<label>Email</label>
		<input id="formEmail" type="email" placeholder="email"/>
		<br />
		<label>Edad</label>
		<input id="formAge" min="18" max="99" type="number"/>		
		<br />
        <button>Enviar</button>
        <button onclick="storeUserData()">Guardar</button>
        <button onclick="getUserData()">Recuperar</button>
    </form>
  </body>
</html>
```

Y el Javascript tal que:

```javascript
function storeUserData() {
	console.log("storeUserData");
	event.preventDefault();
	
	var name = document.getElementById("formNombre").value;
	var email = document.getElementById("formEmail").value;

	var userData = {
		"name": name,
		"email": email
	};

    localStorage.setItem("userDataJSON", JSON.stringify(userData));
}

function getUserData() {
	console.log("getUserData");
	event.preventDefault();

	var userData = JSON.parse(localStorage.getItem("userDataJSON"));
	
	document.getElementById("formNombre").value = userData.name;
	document.getElementById("formEmail").value = userData.email;	
}
```
