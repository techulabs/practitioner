summary: Operaciones CRUD MongoDB
id: CDEX02
categories: data
tags: back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# CDEX02 - Operaciones CRUD MongoDB
<!-- ------------------------ -->
## Overview 
Duration: 60

### CDEX02 - Operaciones CRUD MongoDB - 60 min

**CDEX02 - Operaciones CRUD MongoDB**

Prueba los siguientes comandos en Robo 3T u otro cliente para conectarte a Mongo DB.

Parámetros de conexión > localhost:27017

1. Una vez conectados al servidor de Mongo DB con los parámetros de conexión anteriores, crear una base de datos en un cliente (Robo 3T por ejemplo) y así iremos probando desde cero los principales comandos y lenguaje de Mongo DB.

2. Ahora crearemos una colección "producto":

   `db.createCollection("producto")`

   Las operaciones de creación o inserción agregan nuevos documentos a una colección. Si la colección no existe, las propia operación de inserción creará la colección.

3. MongoDB proporciona los métodos **insertOne()** y insertMany() para insertar nuevos documentos en una colección. Vamos a insertar el primer documento en la colección "producto" creada previamente.

   `db.producto.insertOne(`

   ​	`{`

   ​		`proId: "001",`

   ​		`status: "pending",`

   ​		`order: 2`

   `})`

   Si no creamos una clave primaria **_id** Mongo DB lo hará automáticamente por nosotros ;)

4. En este paso vamos a insertar varios documentos con **insertMany()**:

   ```javascript
   db.collection('producto').insertMany([
     {
       proId: '002',
       qty: 25,
       tags: ['blank', 'red'],
       size: { h: 14, w: 21, uom: 'cm' }
     },
     {
       proId: '003',
       qty: 85,
       tags: ['gray'],
       size: { h: 27.9, w: 35.5, uom: 'm' }
     },
     {
       proId: '004',
       qty: 25,
       tags: ['gel', 'blue'],
       size: { h: 19, w: 22.85, uom: 'cm' }
     }
   ])
   ```

5. Vamos a actualizar la estructura de la colección añadiendo un campo 'date' a todos los documentos. Para ello utilizaremos el operador **$set** junto con la operación **updateMany()**:

```javascript
db.producto.updateMany(
   { },
   { $set: {"date": new Date() } }
 )  
```

7. Para atualizar todos los campos del documento con proId '003' podemos usar la operación **update():**

```javascript
db.producto.update(
   { proId: '003' },
   { $set: 
   		{
   		qty: 500,
        instock: true,
        tags: [ "coats", "outerwear", "clothing" ],
        size: { h: 19, w: 22.85, uom: 'cm' },
    	}
   }
 )     
```

8. Para especificar un campo en un documento con un array anidado en un array usaremos la notación punto (dot notation).

   ```
   db.producto.update(
      { proId: '004' },
      { $set:
         {
           "tags.0": "rain gear",
           "size.w": 22
         }
      }
   )
   ```

9. En este paso vamos a utilizar el homólogo de la operación *Select* de SQL: **find()**, el cual nos permitirá hacer búsquedas y filtrar documentos.

```
db.producto.find(
	{ qty:
    	{ $lt : 50 }
	}
)
```

9. Ahora con el operador **$and** filtramos por dos criterios:

```
db.producto.find({
  $and: [{ qty: { $gt: 30 } }, { qty: { $lt: 100 } }]
});
```

REFERENCIAS:

https://docs.mongodb.com/manual/crud/