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

## Grabar datos

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
