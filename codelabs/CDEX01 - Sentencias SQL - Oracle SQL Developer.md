summary: Sentencias SQL - Oracle SQL Developer
id: CDEX01
categories: data
tags: back
status: Published 
authors: TechU
Feedback Link: http://bbva-techuniversity.appspot.com/

# CDEX01 - Sentencias SQL - Oracle SQL Developer
<!-- ------------------------ -->
## Overview 
Duration: 60

### CDEX01 - Sentencias SQL - Oracle SQL Developer - 60 min

**CDEX01 - Sentencias SQL - Oracle SQL Developer**

Para esta práctica contaremos con un cliente (Oracle SQL Developer) de bases de datos relacional y nos conectaremos con los siguientes parámetros:  *localhost:1521*

1. Crearemos una tabla para probar diversas **sentencias SQL** relacionadas con acciones a nivel de tabla.

```sql
CREATE TABLE persona (
    userid int PRIMARY KEY,
    last_name varchar(255) NOT NULL,
    first_name varchar(255),
    age int
);
```

2. Vamos a insertar un registro nuevo en la tabla 'persona' que acabamos de crear:

   ```sql
   INSERT INTO persona (userid, last_name, age)
   VALUES (100, 'Acosta', 38);
   ```

   Y comprobamos la insercción:

   ```sql
   SELECT * FROM persona; 
   ```

   3/ Para probar una de las operaciones más comunes en bases de datos relacionales, **JOIN**, necesitamos crear otra tabla:

   ```sql
   CREATE TABLE pedido (
       pedidoID int PRIMARY KEY,
       total int,
       userid int
   );
   ```

   Insertamos tres pedidos hechos por la misma persona (userid=100):

   ```sql
   INSERT INTO pedido (pedidoID, userid, total)
   VALUES (1, 100, 1500);
   
   INSERT INTO pedido (pedidoID, userid, total)
   VALUES (2, 100, 500);
   
   INSERT INTO pedido (pedidoID, userid, total)
   VALUES (3, 100, 1000);
   ```

   4/ La siguiente setencia SQL devolverá todos los pedidos que tengan información del cliente en el campo 'userid'. Para ello, usaremos **INNER JOIN** con el que se seleccionan los registros que tienen valores coincidentes en ambas tablas:

   ```sql
   SELECT pedido.pedidoID, persona.last_name
   FROM pedido
   INNER JOIN persona ON pedido.userid = persona.userid;
   ```

REFERENCIAS:

- https://www.w3schools.com/sql/default.asp
- https://docs.mongodb.com/manual/reference/sql-comparison/#terminology-and-concepts