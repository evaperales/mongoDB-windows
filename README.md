# MongoDB - Instalación y primeros pasos (Windows)

## Introducción a MongoDB

MongoDB es una base de datos NoSQL. En ella  no tenemos tablas, no tenemos filas de datos y no tenemos SQL. Pero MongoDB nos sirve igualmente para almacenar los datos de nuestras aplicaciones. 

MongoDB es una base de datos orientada a documentos. Lo que quiere decir que en lugar de guardar los datos en las filas de tablas, guarda los datos en documentos. Estos documentos son almacenados en BSON, que es una representación binaria de JSON.

Un documento es un concepto similar a un objeto, y una colección a una tabla de una base de datos relacional. Los documentos se almacenan en una colección. Pero los documentos que almacena una colección no tienen por qué tener la misma estructura, es decir, no tienen que contener el mismo número y tipo de campos. Esto es impensable en una base de datos relacional.

MongoDB se puede utilizar en muchos de los proyectos actuales. Por ejemplo, en las aplicaciones CRUD o en muchos de los desarrollos web. Cualquier aplicación que necesite almacenar datos semi estructurados puede usar MongoDB. 

Aunque las colecciones de MongoDB no necesitan un modelo hay que tener claro si los datos deben estar normalizados, denormalizarlos o utilizar una aproximación híbrida. Estas decisiones pueden afectar al rendimiento de nuestra aplicación. En definitiva el esquema lo definen las consultas que vayamos a realizar con más frecuencia.

MongoDB es especialmente útil en aplicaciones que requieran escalabilidad.

Sin embargo, en MongoDB no existen las transacciones. Solo garantiza operaciones atómicas a nivel de documento. Si las transacciones son algo indispensable en nuestro desarrollo, deberemos pensar en otro sistema.Tampoco existen los JOINS. Para consultar datos relacionados en dos o más colecciones, tenemos que hacer más de una consulta. En general, si nuestros datos pueden ser estructurados en tablas, y necesitamos las relaciones, es mejor que optemos por un sistema de base de datos relacional.

## Instalación de MongoDB

1. Ir a la [página oficial de MongoDB](https://www.mongodb.com/es)
2. Descargar Enterprise Server para Windows x64.
3. Descomprimir.
4. Cambiar el nombre a la carperta descomprimida para que se llame mongodb y moverla a C. Comprueba que tienes c:\mongodb\bin.
5. Crea la carpeta c:\data\db.

## Arrancar el servicio

```console
c:\mongodb\bin\mongod.exe 
```
## Ejecutar el interfaz de línea de comandos de MongoDB

```console
c:\mongodb\bin\mongo.exe
```
 ## Ver bases de datos existentes
 
  ```console
 > show databases
admin  0.000GB
config 0.000GB
local  0.000GB
```
Tambien podemos poner show dbs  

## Usar una base de datos (existente o no)

```console
> use gestion
switched to db gestion
```

## Ver en qué base de datos nos encontramos

```console
> db
gestion
```

## Borrar una base de datos

```console
> use prueba
switched to db prueba
> db.dropDatabase()
{ "ok" : 1 }
```

## Crear objetos (documentos)

```console
> use gestion
switched to db gestion
> var persona1 = {nombre: "Mario", apellido: "Neta"}
> var persona2 = {nombre: "Pere", apellido: "Gil", pais: "España"}
> persona1
{ "nombre" : "Mario", "apellido" : "Neta" }
> persona2
{ "nombre" : "Pere", "apellido" : "Gil", "pais" : "España" }
```

## Insertar datos (Documentos)

Vamos a grabar datos en la colección `agenda` dentro de la base de datos `gestion`. Una colección es el equivalente a una tabla en las bases de datos relacionales.

```console
> db.agenda.insert(persona1)
WriteResult({ "nInserted" : 1 })
> db.agenda.insert(persona2)
WriteResult({ "nInserted" : 1 })
> db.agenda.insert({nombre: "Elba", apellido: "Lazo", edad: 24})
WriteResult({ "nInserted" : 1 })
```

Observa que los documentos insertados (los datos sobre personas) no tienen por qué ajustarse a la misma estructura, pueden ser heterogéneos y, por tanto, contener distinto número y tipo de campos.

## Ver las colecciones de la base de datos actual

```console
> show collections
agenda
```

## Ver el contenido de una colección

Sería el equivalente a `SELECT * FROM usuarios` en una base de datos relacional.

```console
> db.agenda.find()
{ "_id" : ObjectId("58937be7a70c3985de49a38f"), "nombre" : "Mario", "apellido" : "Neta" }
{ "_id" : ObjectId("58937beca70c3985de49a390"), "nombre" : "Pere", "apellido" : "Gil", "pais" : "España" }
{ "_id" : ObjectId("58937c23a70c3985de49a391"), "nombre" : "Elba", "apellido" : "Lazo", "edad" : 24 }
```

Observa que a cada elemento insertado se le asigna de forma automática un identificador único.

## Mostrar un solo documento

```console
> db.agenda.findOne()
{
	"_id" : ObjectId("58937be7a70c3985de49a38f"),
	"nombre" : "Mario",
	"apellido" : "Neta"
}
```

## Búsqueda condicional I

```console
> db.agenda.find({nombre: "Elba"})
{ "_id" : ObjectId("58937c23a70c3985de49a391"), "nombre" : "Elba", "apellido" : "Lazo", "edad" : 24 }
> 
> db.agenda.find({pais: "España"})
{ "_id" : ObjectId("58937beca70c3985de49a390"), "nombre" : "Pere", "apellido" : "Gil", "pais" : "España" }
```

## Búsqueda condicional II

`$ne` significa *not equal*, `$gt` es *greater than* y `$lt` es *less than*.

```console
> db.agenda.find( { nombre: {$ne: "Elba"} } )
{ "_id" : ObjectId("58937be7a70c3985de49a38f"), "nombre" : "Mario", "apellido" : "Neta" }
{ "_id" : ObjectId("58937beca70c3985de49a390"), "nombre" : "Pere", "apellido" : "Gil", "pais" : "España" }
> 
> db.agenda.find( { pais: {$ne: "España"} } )
{ "_id" : ObjectId("58937be7a70c3985de49a38f"), "nombre" : "Mario", "apellido" : "Neta" }
{ "_id" : ObjectId("58937c23a70c3985de49a391"), "nombre" : "Elba", "apellido" : "Lazo", "edad" : 24 }
> 
> db.agenda.find( { edad: {$gt: 18} } )
{ "_id" : ObjectId("58937c23a70c3985de49a391"), "nombre" : "Elba", "apellido" : "Lazo", "edad" : 24 }
> 
> db.agenda.find( { edad: {$lt: 18} } )
```

## Insertar varios documentos al mismo tiempo

Mediante `insert` se puede insertar un elemento o bien un array con varios elementos.

```console
> var p1 = { nombre: "Lola", apellido: "Mento", edad: 35 }
> var p2 = { nombre: "Encarna", apellido: "Vales", edad: 17, pais: "USA" }
> db.agenda.insert( [p1, p2] )
BulkWriteResult({
	"writeErrors" : [ ],
	"writeConcernErrors" : [ ],
	"nInserted" : 2,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"upserted" : [ ]
})
> db.agenda.find()
{ "_id" : ObjectId("58937be7a70c3985de49a38f"), "nombre" : "Mario", "apellido" : "Neta" }
{ "_id" : ObjectId("58937beca70c3985de49a390"), "nombre" : "Pere", "apellido" : "Gil", "pais" : "España" }
{ "_id" : ObjectId("58937c23a70c3985de49a391"), "nombre" : "Elba", "apellido" : "Lazo", "edad" : 24 }
{ "_id" : ObjectId("58938745a70c3985de49a392"), "nombre" : "Lola", "apellido" : "Mento", "edad" : 35 }
{ "_id" : ObjectId("58938745a70c3985de49a393"), "nombre" : "Encarna", "apellido" : "Vales", "edad" : 17, "pais" : "USA" }
```

## Edición de un documento con `save()`

```console
> var persona = db.agenda.findOne({ "_id" : ObjectId("58938745a70c3985de49a392")})
> persona
{
	"_id" : ObjectId("58938745a70c3985de49a392"),
	"nombre" : "Lola",
	"apellido" : "Mento",
	"edad" : 35
}
> persona.nombre = "Salva"
Salva
> persona
{
	"_id" : ObjectId("58938745a70c3985de49a392"),
	"nombre" : "Salva",
	"apellido" : "Mento",
	"edad" : 35
}
> db.agenda.save(persona)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.agenda.find()
{ "_id" : ObjectId("58937be7a70c3985de49a38f"), "nombre" : "Mario", "apellido" : "Neta" }
{ "_id" : ObjectId("58937beca70c3985de49a390"), "nombre" : "Pere", "apellido" : "Gil", "pais" : "España" }
{ "_id" : ObjectId("58937c23a70c3985de49a391"), "nombre" : "Elba", "apellido" : "Lazo", "edad" : 24 }
{ "_id" : ObjectId("58938745a70c3985de49a392"), "nombre" : "Salva", "apellido" : "Mento", "edad" : 35 }
{ "_id" : ObjectId("58938745a70c3985de49a393"), "nombre" : "Encarna", "apellido" : "Vales", "edad" : 17, "pais" : "USA" }
```

:warning: Cuidado al guardar el documento en una variable. Hay que usar `findOne()` ya que utilizando `find()` el valor de la variable se pierde. Otra opción es volcar el resultado de la consulta en un array con `find().toArray`.

## Edición de un documento con `update()`

Vamos a modificar la edad de la persona cuyo nombre es `Encarna`.

```console
> p = db.agenda.findOne({nombre: "Encarna"})
{
	"_id" : ObjectId("58938745a70c3985de49a393"),
	"nombre" : "Encarna",
	"apellido" : "Vales",
	"edad" : 17,
	"pais" : "USA"
}
> p.edad = 18
18
> db.agenda.update({nombre: "Encarna"}, p)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.agenda.find({nombre: "Encarna"})
{ "_id" : ObjectId("58938745a70c3985de49a393"), "nombre" : "Encarna", "apellido" : "Vales", "edad" : 18, "pais" : "USA" }
```

## Edición de un documento con `update()` y `$set`

De nuevo vamos a modificar la edad de `Encarna`.

```console
> db.agenda.update( {nombre: "Encarna"}, {$set: {edad: 19} } )
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.agenda.find({nombre: "Encarna"})
{ "_id" : ObjectId("58938745a70c3985de49a393"), "nombre" : "Encarna", "apellido" : "Vales", "edad" : 19, "pais" : "USA" }
```

## Eliminar documentos

Vamos a eliminar todos los personas cuyo atributo `pais` sea `España`.

```console
> db.agenda.find()
{ "_id" : ObjectId("58937be7a70c3985de49a38f"), "nombre" : "Mario", "apellido" : "Neta" }
{ "_id" : ObjectId("58937beca70c3985de49a390"), "nombre" : "Pere", "apellido" : "Gil", "pais" : "España" }
{ "_id" : ObjectId("58937c23a70c3985de49a391"), "nombre" : "Elba", "apellido" : "Lazo", "edad" : 24 }
{ "_id" : ObjectId("58938745a70c3985de49a392"), "nombre" : "Salva", "apellido" : "Mento", "edad" : 35 }
{ "_id" : ObjectId("58938745a70c3985de49a393"), "nombre" : "Encarna", "apellido" : "Vales", "edad" : 19, "pais" : "USA" }
> 
> db.agenda.remove({pais: "España"})
WriteResult({ "nRemoved" : 1 })
> 
> db.agenda.find()
{ "_id" : ObjectId("58937be7a70c3985de49a38f"), "nombre" : "Mario", "apellido" : "Neta" }
{ "_id" : ObjectId("58937c23a70c3985de49a391"), "nombre" : "Elba", "apellido" : "Lazo", "edad" : 24 }
{ "_id" : ObjectId("58938745a70c3985de49a392"), "nombre" : "Salva", "apellido" : "Mento", "edad" : 35 }
{ "_id" : ObjectId("58938745a70c3985de49a393"), "nombre" : "Encarna", "apellido" : "Vales", "edad" : 19, "pais" : "USA" }

```

## Consultar un número concreto de campos

Vamos a mostrar un listado de las personas de la agenda solo con los campos `nombre` y `edad`.

```console
> db.agenda.find({}, {nombre: 1, edad: 1, _id:0})
{ "nombre" : "Mario" }
{ "nombre" : "Elba", "edad" : 24 }
{ "nombre" : "Salva", "edad" : 35 }
{ "nombre" : "Encarna", "edad" : 19 }
```

## Contar documentos

```console
> db.agenda.find()
{ "_id" : ObjectId("58937be7a70c3985de49a38f"), "nombre" : "Mario", "apellido" : "Neta" }
{ "_id" : ObjectId("58937c23a70c3985de49a391"), "nombre" : "Elba", "apellido" : "Lazo", "edad" : 24 }
{ "_id" : ObjectId("58938745a70c3985de49a392"), "nombre" : "Salva", "apellido" : "Mento", "edad" : 35 }
{ "_id" : ObjectId("58938745a70c3985de49a393"), "nombre" : "Encarna", "apellido" : "Vales", "edad" : 19, "pais" : "USA" }
> 
> db.usuarios.find().count()
4
```

## Consultas ordenadas

El valor `1` se utiliza para realizar una consulta en orden ascendente y el `-1` para descendente. Con `limit()` se puede limitar el resultado a un número máximo de documentos.

```console
> db.agenda.find().sort( {apellido: 1} )
{ "_id" : ObjectId("58937c23a70c3985de49a391"), "nombre" : "Elba", "apellido" : "Lazo", "edad" : 24 }
{ "_id" : ObjectId("58938745a70c3985de49a392"), "nombre" : "Salva", "apellido" : "Mento", "edad" : 35 }
{ "_id" : ObjectId("58937be7a70c3985de49a38f"), "nombre" : "Mario", "apellido" : "Neta" }
{ "_id" : ObjectId("58938745a70c3985de49a393"), "nombre" : "Encarna", "apellido" : "Vales", "edad" : 19, "pais" : "USA" }
> 
> db.agenda.find().sort( {apellido: -1} )
{ "_id" : ObjectId("58938745a70c3985de49a393"), "nombre" : "Encarna", "apellido" : "Vales", "edad" : 19, "pais" : "USA" }
{ "_id" : ObjectId("58937be7a70c3985de49a38f"), "nombre" : "Mario", "apellido" : "Neta" }
{ "_id" : ObjectId("58938745a70c3985de49a392"), "nombre" : "Salva", "apellido" : "Mento", "edad" : 35 }
{ "_id" : ObjectId("58937c23a70c3985de49a391"), "nombre" : "Elba", "apellido" : "Lazo", "edad" : 24 }
> 
> db.agenda.find().sort( {apellido: -1} ).limit(3)
{ "_id" : ObjectId("58938745a70c3985de49a393"), "nombre" : "Encarna", "apellido" : "Vales", "edad" : 19, "pais" : "USA" }
{ "_id" : ObjectId("58937be7a70c3985de49a38f"), "nombre" : "Mario", "apellido" : "Neta" }
{ "_id" : ObjectId("58938745a70c3985de49a392"), "nombre" : "Salva", "apellido" : "Mento", "edad" : 35 }
```


## La función `skip()`

La función `skip()` permite "saltar" un número determinado de documentos de la consulta.

```console
> db.agenda.find().sort( {apellido: 1} ).limit(2)
{ "_id" : ObjectId("58937c23a70c3985de49a391"), "nombre" : "Elba", "apellido" : "Lazo", "edad" : 24 }
{ "_id" : ObjectId("58938745a70c3985de49a392"), "nombre" : "Salva", "apellido" : "Mento", "edad" : 35 }
> 
> db.agenda.find().sort( {apellido: 1} ).skip(1).limit(2)
{ "_id" : ObjectId("58938745a70c3985de49a392"), "nombre" : "Salva", "apellido" : "Mento", "edad" : 35 }
{ "_id" : ObjectId("58937be7a70c3985de49a38f"), "nombre" : "Mario", "apellido" : "Neta" }
```

## La función `size()`

A diferencia de `count()`, el método `size()` ofrece la cuenta de la consulta una vez filtrada con `skip()`, `limit()`, etc.

```console
> db.agenda.find().sort( {apellido: 1} ).skip(1).limit(2).count()
4
> 
> db.agenda.find().sort( {apellido: 1} ).skip(1).limit(2).size()
2
```


## Agrupación de documentos

Antes de hacer pruebas con la agrupación de documentos, vamos a meter más datos en nuestra colección de agenda.

```console
> db.agenda.insert({nombre: "Elsa", apellido: "Pato", edad: 52, pais: "Portugal"})
WriteResult({ "nInserted" : 1 })
> db.agenda.insert({nombre: "Armando", apellido: "Bronca", edad: 22, pais: "Francia"})
WriteResult({ "nInserted" : 1 })
> db.agenda.insert({nombre: "Leandro", apellido: "Gado", edad: 48, pais: "Venezuela"})
WriteResult({ "nInserted" : 1 })
> db.agenda.insert({nombre: "Olga", apellido: "Seosa", edad: 29, pais: "España"})
WriteResult({ "nInserted" : 1 })
> db.agenda.insert({nombre: "Elena", apellido: "Nito", edad: 30, pais: "USA"})
WriteResult({ "nInserted" : 1 })
> 
> db.agenda.find()
{ "_id" : ObjectId("58937be7a70c3985de49a38f"), "nombre" : "Mario", "apellido" : "Neta" }
{ "_id" : ObjectId("58937beca70c3985de49a390"), "nombre" : "Pere", "apellido" : "Gil", "pais" : "España" }
{ "_id" : ObjectId("58937c23a70c3985de49a391"), "nombre" : "Elba", "apellido" : "Lazo", "edad" : 24 }
{ "_id" : ObjectId("58938745a70c3985de49a392"), "nombre" : "Salva", "apellido" : "Mento", "edad" : 35 }
{ "_id" : ObjectId("58938745a70c3985de49a393"), "nombre" : "Encarna", "apellido" : "Vales", "edad" : 17, "pais" : "USA" }
{ "_id" : ObjectId("5895b74415c260814ec7f139"), "nombre" : "Elsa", "apellido" : "Pato", "edad" : 52, "pais" : "Portugal" }
{ "_id" : ObjectId("5895b77215c260814ec7f13a"), "nombre" : "Armando", "apellido" : "Bronca", "edad" : 22, "pais" : "Francia" }
{ "_id" : ObjectId("5895b7d915c260814ec7f13b"), "nombre" : "Leandro", "apellido" : "Gado", "edad" : 48, "pais" : "Venezuela" }
{ "_id" : ObjectId("5895b88815c260814ec7f13c"), "nombre" : "Olga", "apellido" : "Seosa", "edad" : 29, "pais" : "España" }
{ "_id" : ObjectId("5895b8ab15c260814ec7f13d"), "nombre" : "Elena", "apellido" : "Nito", "edad" : 30, "pais" : "USA" }
```

Vamos a mostrar todos los países de donde son las personas de la agenda.

```console
> db.agenda.aggregate( [ {$group: {_id: "$pais"}} ] )
{ "_id" : "Venezuela" }
{ "_id" : "Francia" }
{ "_id" : null }
{ "_id" : "España" }
{ "_id" : "USA" }
{ "_id" : "Portugal" }
```

Ahora igual pero diciendo cuántas veces se repite cada pais.

```console
> db.agenda.aggregate( [ {$group: {_id: "$pais", repetidos: {$sum: 1}}} ] )
{ "_id" : "Venezuela", "repetidos" : 1 }
{ "_id" : "Francia", "repetidos" : 1 }
{ "_id" : null, "repetidos" : 3 }
{ "_id" : "España", "repetidos" : 2 }
{ "_id" : "USA", "repetidos" : 2 }
{ "_id" : "Portugal", "repetidos" : 1 }
```

Igual y además con la media de edad por pais.

```console
> db.agenda.aggregate( [ {$group: {_id: "$pais", repetidos: {$sum: 1}, "edad media": {$avg: "$edad"}}} ] )
{ "_id" : "Venezuela", "repetidos" : 1, "edad media" : 48 }
{ "_id" : "Francia", "repetidos" : 1, "edad media" : 22 }
{ "_id" : null, "repetidos" : 3, "edad media" : 29.5 }
{ "_id" : "España", "repetidos" : 2, "edad media" : 29 }
{ "_id" : "USA", "repetidos" : 2, "edad media" : 23.5 }
{ "_id" : "Portugal", "repetidos" : 1, "edad media" : 52 }
```

Igual que todo lo anterior y además excluyendo los valores `null` para el atributo `pais`.

```console
> db.agenda.aggregate( [ {$match: {pais: {$ne: null}}}, {$group: {_id: "$pais", repetidos: {$sum: 1}, "edad media": {$avg: "$edad"}}} ])
{ "_id" : "Venezuela", "repetidos" : 1, "edad media" : 48 }
{ "_id" : "España", "repetidos" : 2, "edad media" : 29 }
{ "_id" : "USA", "repetidos" : 2, "edad media" : 23.5 }
{ "_id" : "Portugal", "repetidos" : 1, "edad media" : 52 }
{ "_id" : "Francia", "repetidos" : 1, "edad media" : 22 }
```


## Consultas con expresiones regulares

Vamos a mostrar todos los usuarios cuyos apellidos contienen la letra "e".

```console
> db.agenda.find( {apellido: /e/} )
{ "_id" : ObjectId("58937be7a70c3985de49a38f"), "nombre" : "Mario", "apellido" : "Neta" }
{ "_id" : ObjectId("58938745a70c3985de49a392"), "nombre" : "Salva", "apellido" : "Mento", "edad" : 35 }
{ "_id" : ObjectId("58938745a70c3985de49a393"), "nombre" : "Encarna", "apellido" : "Vales", "edad" : 17, "pais" : "USA" }
{ "_id" : ObjectId("5895b88815c260814ec7f13c"), "nombre" : "Olga", "apellido" : "Seosa", "edad" : 29, "pais" : "España" }
```

Usuarios cuyo nombre termina con la cadena "na".

```console
> db.agenda.find( {nombre: /na$/} )
{ "_id" : ObjectId("58938745a70c3985de49a393"), "nombre" : "Encarna", "apellido" : "Vales", "edad" : 17, "pais" : "USA" }
{ "_id" : ObjectId("5895b8ab15c260814ec7f13d"), "nombre" : "Elena", "apellido" : "Nito", "edad" : 30, "pais" : "USA" }
```

Usuarios cuyo nombre comienza por "El".

```console
> db.agenda.find( {nombre: /^El/} )
{ "_id" : ObjectId("58937c23a70c3985de49a391"), "nombre" : "Elba", "apellido" : "Lazo", "edad" : 24 }
{ "_id" : ObjectId("5895b74415c260814ec7f139"), "nombre" : "Elsa", "apellido" : "Pato", "edad" : 52, "pais" : "Portugal" }
{ "_id" : ObjectId("5895b8ab15c260814ec7f13d"), "nombre" : "Elena", "apellido" : "Nito", "edad" : 30, "pais" : "USA" }
> 
```


## Ver los documentos en un formato "bonito"

El método `pretty()` nos permite ver los documentos de una manera bonita y legible. A continuación se compara una consulta sin `pretty()` y otra con `pretty()`.

```console
> db.agenda.find().limit(3)
{ "_id" : ObjectId("58937be7a70c3985de49a38f"), "nombre" : "Mario", "apellido" : "Neta" }
{ "_id" : ObjectId("58937c23a70c3985de49a391"), "nombre" : "Elba", "apellido" : "Lazo", "edad" : 24 }
{ "_id" : ObjectId("58938745a70c3985de49a392"), "nombre" : "Salva", "apellido" : "Mento", "edad" : 35 }
> 
> db.agenda.find().limit(3).pretty()
{
	"_id" : ObjectId("58937be7a70c3985de49a38f"),
	"nombre" : "Mario",
	"apellido" : "Neta"
}
{
	"_id" : ObjectId("58937c23a70c3985de49a391"),
	"nombre" : "Elba",
	"apellido" : "Lazo",
	"edad" : 24
}
{
	"_id" : ObjectId("58938745a70c3985de49a392"),
	"nombre" : "Salva",
	"apellido" : "Mento",
	"edad" : 35
}
```


## Crear colecciones con validación de atributos

Al crear una colección, se puede indicar a MongoDB que los atributos sean de un tipo concreto o que cumplan con un determinado patrón.

Vamos a crear una nueva colección llamada `empleado` cuyo atributo nombre será de tipo `string`, `sueldo` será de tipo `number` y `email` deberá contener un símbolo `@`.

```console
db.createCollection("empleado", {
  validator: {
    $and: [
      { nombre: {$type: "string"} },
      { sueldo: {$type: "number"} },
      { email: {$regex: /@/} }
    ]
  }
})
```


## Crear índices

```console
> db.agenda.createIndex({nombre: 1})
{
	"createdCollectionAutomatically" : false,
	"numIndexesBefore" : 1,
	"numIndexesAfter" : 2,
	"ok" : 1
}
```


