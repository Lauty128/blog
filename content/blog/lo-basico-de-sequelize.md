---
title: Lo básico de Sequelize
date: "2023-03-28"
description: "Sequelize es un ORM que permite a los usuarios llamar a funciones javascript para interactuar con SQL DB sin escribir consultas reales. Es bastante útil para acelerar el tiempo de desarrollo."
tags: ["Programación"]
---

# Sequelize
Sequelize es un ORM que permite a los usuarios llamar a funciones javascript para interactuar con SQL DB sin escribir consultas reales. Es bastante útil para acelerar el tiempo de desarrollo.

Cuando lo importamos, debemos hacerlo con `Sequelize`, ya que `sequelize` esta reservado para instanciar el objeto. Todo esto segun la documentacion oficial

``` javascript
import Sequelize from 'sequelize';

const sequelize = new Sequelize(...)
```


## **índice**
* [Conexión](#conexion)
    * [authenticate()](#authenticate)
* [Schemas](#schemas)
    * [define()](#define)
    * [hasMany()](#hasmany)
* [Queries](#queries)
    * [create()](#create)
    * [findAll()](#findall)
    * [finders](#finders)
    * [destroy()](#destroy)
    * [update()](#update)
* [Manejo de errores](#manejo-de-errores)
* [Opinión](#opinión)

---

## Conexión
Ya se vio arriba como sé instancia el objeto, por lo que ahora, con esa instancia, vamos a ver como se conecta a la base de datos.    
`Sequelize` pide 3 parametros principales `(db:String, user:String, password:String, options:Object)`   
En el apartado de opciones debemos colocar dos propiedades, una es `host`, donde vamos a especificar el host de la base de datos, y la otra es `dialect`, el cual acepta 1 de 4 opciones 'mysql' | 'postgre' | 'mariadb' | 'mssql'   

``` javascript
const sequelize = new Sequelize('mydb', 'user','password',{
    host:'localhost',
    dialect: 'mysql'   // other options = 'mssql' | 'mariadb' | 'postgre' 
})
```

Ahora, que ya tenemos la configuración hecha, debemos conectarnos. Eso lo hacemos con el método `sync()`.   
Se recomienda encerrar él listen de la api en una función asíncrona y ejecutarlo dentro de un try & catch por si ocurre un error durante la conexion.

``` javascript
import { sequelize } from './config/dabatabase.js';

async function main(){
    try {
        await sequelize.sync();
        console.log('Connection has been established successfully.');
        app.listen(PORT, ()=>{
            console.log('Server on in port ' + PORT)
        })
    }catch (error) {
        console.error('Unable to connect to the database:', error);
    }
}
```
> `sync()` puede tomar como parametro, `{ force:true }` o `{ alter:true }`. Esto genera un cambio en la forma en que se lee la base de datos al iniciar la aplicacion, por lo que recomiendo leer la [documentación](https://sequelize.org/docs/v6/core-concepts/model-basics/#database-safety-check)

### **authenticate()**
Con este metodo de `sequelize` se puede probar si funciona la conexion con la base de datos. Simplemente se reemplaza `sync()` con `authenticate()` y se analiza que devuelve como respuesta.

``` javascript
async function main(){
    try {
        await sequelize.authenticate();
        console.log('Connection has been established successfully.');
        app.listen(PORT, ()=>{
            console.log('Server on in port ' + PORT)
        })
    } catch (error) {
        console.error('Unable to connect to the database:', error);
    }
}
```

**[⬆ Volver a índice](#indice)** 

---

## Schemas
Para usar una estructura sencilla vamos a crear una carpeta `models` y dentro vamos a colocar los archivos .js de cada Schema.
Dentro de los mismos vamos a crear los Schemas, y para eso primero vamos a importar dos cosas fundamentales.

``` javascript
import { DataType } from 'sequelize';
import { sequelize } from './config/dabatabase.js';
```
Llamamos a la configuración que hicimos en el archivo `database.js` y a un método llamado `DataType`, el cual nos permite obtener los tipos de datos de sequelize para utilizarlos en nuestros Schemas  

### **define()**
Con este método de `sequelize` vamos a definir los Schemas. Recibe dos parámetros, primero el nombre del modelo(en plural, a diferencia de mongoose) y segundo el modelo de la tabla.  
Para utilizar esta tabla debemos exportarla.

``` javascript
export const Users = sequelize.define('users',{
    id:{
        type: DataType.STRING(10),
        primaryKey: true,
        defaultValue: generatePassword()  //- Function of the APP
    }
    name: DataType.STRING,
    age: DataType.INTEGER,
    date:{
        type:DataType.DATE()
        defaultValue: new Date()
    }
})
```
> Lo bueno que tiene sequelize es que agrega dos campos más a tu tabla, los cuales son `createdAt` y `updatedAt`, para saber cuando se creó y cuando fue la última vez que se modificó el documento. Además, Todo eso lo hace automático.

Para ver todos los tipos de datos de `DataTypes` les recomiendo ver la [documentación](https://sequelize.org/docs/v6/core-concepts/model-basics/#data-types).

**[⬆ Volver a índice](#indice)** 

---

### **hasMany()**
Este método es bastante importante, ya que a través de este método vamos a definir las relaciones de las tablas, que serian las foreign keys.   
Para explicar esto vamos a poner de ejemplo una base de datos que tiene una tabla de `Proyectos` y otro de `Tareas`. La relación que tienen estas tablas se encuentra en el id de un Proyecto, ya que dependiendo la tarea, esta va a contener, además de su id único, otro id que representa al proyecto al que pertenece.  

```javascript
import { sequelize } '../config/dabatabase.js';
import { Task } from './Task.js';

export const Project = sequelize.define('Projects', {
    id:{...},
    name:{...},
    description:{...}
})

Project.hasMany(Task)
```
> Podría explicarse como: "Un proyecto tiene varias tareas".   

Esto lo que hará es crear una propiedad extra en la tabla `Task` que se llamara, en este caso, `ProjectId`. Esta nueva propiedad será la foreign key que se asocia al proyecto correspondiente.  

> Existe otro método llamado `belongsTo()` que, básicamente, sería lo mismo que `hasMany()` pero al revés.

**[⬆ Volver a índice](#indice)** 

---

## Queries
Las consultas son bastantes sencillas de hacer. Pueden tomar cierto grado de dificultas a la hora de añadir filtros a la query, pero si la API no es demasiado grande, se hace bastante sencillo de manejar.  
Aqui quiero comentar un poco sobre los metodos basicos para comenzar a utilizarlos.

### **create()**
Como el nombre lo indica, con este métodos puede crear un nuevo objeto para la tabla especificada.   
Como primer parámetro recibe un objeto con las propiedades necesarias para poder agregar el nuevo objeto y como segundo parámetro recibe las opciones. 
Vale la pena darle un vistazo a las [opciones](https://sequelize.org/api/v6/class/src/model.js~model#static-method-create), ya que tiene bastantes interesantes como `validate:false` que desactiva las validaciones o `silent:true` que, al tenerlo activo, sequelize no actualiza el `updateAt`(Aunque esta opción es más para cuando editamos un objeto, pero también es interesante tenerlo en cuenta)  

``` javascript
import { Task } from '../models/Task.js';

const newTask = async (req,res) => {
    const values = req.body
    const options = {...}

    const tasks = await Task.create(values, options)
    res.json(tasks)
}
```

**[⬆ Volver a índice](#indice)** 

---

### **findAll()**
Permite realizar las consultas a una tabla. Puede ser desde una consulta básica, como se ve en el ejemplo, hasta una consulta compleja con bastantes filtros entre medio.  

``` javascript
import { Task } from '../models/Task.js';

const getTasks = async (req,res) => {
    const options = {...}

    const tasks = await Task.findAll(options)
    res.json(tasks)
}
```
A través de las opciones podemos seleccionar algunos atributos de la tabla o agregar un where, entre muchas otras opciones.  
Les recomiendo leer la [documentación](https://sequelize.org/api/v6/class/src/model.js~model#static-method-findAll) e informarse un poco más sobre las opciones que tiene.

### **Finders**
Está bueno tener en cuenta todos los tipos de finders que tiene sequelize e intentar variar con ellos, dependiendo que tipo de acción tengan que realizar. No es bueno utilizar siempre él `findAll()`.
* [findAll()](https://sequelize.org/docs/v6/core-concepts/model-querying-finders/#findall)
* [findByPk()](https://sequelize.org/docs/v6/core-concepts/model-querying-finders/#findbypk)
* [findOne()](https://sequelize.org/docs/v6/core-concepts/model-querying-finders/#findone)
* [findOrCreate()](https://sequelize.org/docs/v6/core-concepts/model-querying-finders/#findorcreate)
* [findAndCountAll()](https://sequelize.org/docs/v6/core-concepts/model-querying-finders/#findandcountall)

**[⬆ Volver a índice](#indice)** 

---

### **destroy()**
Este método permite eliminar una fila de la tabla. El mismo, igual que `findAll()` recibe una `options` como parámetro y para poder acceder a un objeto específico se implementa un where a la consulta, a través de las `options`.  

```javascript
import { Task } from '../models/Task.js';

const deleteTask = async (req,res) => {
    const id = req.params.id
    const options = { where: { id } }

    const task = await Task.destroy(options)
    res.json(task)
}
```
> En este ejemplo estamos recibiendo el id a través de la url. Accediendo a `req.params` podemos obtener su valor.  

**[⬆ Volver a índice](#indice)** 

---

### **update()**
Este método permite actualizar un objeto. Para eso se debe pasar dos parámetros, primero el `value` que envía los nuevos datos a actualizar y segundo las `options` que, como usamos en [destroy()](#destroy), se le pasa una where con el id del objeto a actualizar.

```javascript
import { Task } from '../models/Task.js';

const updateTask = async (req,res) => {
    const id = req.params.id
    const options = { where: { id } }

    const values = req.body

    const task = await Task.update(values, options)
    res.json(task)
}
```
Tambien existe otra forma de actualizar, pero yo la utilizaria solo en caso de que la tabla tenga pocas propiedades. Es bastante comoda de usar pero, repito, yo solo la utilizaria en caso de tablas muy pequeñas.
La forma es la siguiente

```javascript
import { Task } from '../models/Task.js';

const updateTask = async (req,res) => {
    const id = req.params.id
    const { name, description, project_id  } = req.body

    const task = await Task.findPK(id)
    task.name = name
    task.description = description
    task.project_id = project_id

    await task.save()
    res.json(task)
}
```
> Es una buena forma de actualizar un objeto. Pero que pasaria en caso de que un objeto tenga 10 propiedades o mas.  


**[⬆ Volver a índice](#indice)** 

---

## Manejo de errores
Cuando se ejecuta una consulta, hay que tener en cuenta el manejo de errores, en caso de que los halla. Esto es sumamente importante, ya que en caso de que ocurra un error antes o durante la ejecución de la consulta, si no existe un manejo de los errores, el programa se detendrá.   
Lo más común seria hacer un try & catch para solucionar ese problema. Lo bueno de esto es que se puede crear un mensaje personalizado para devolver, en caso de que haya un error.

``` javascript
import { Task } from '../models/Task.js';

const getTasks = async (req,res) => {
    try{
        const tasks = await Task.findAll()
        res.json(tasks)

    } catch(error){
        res.status(500).json({
            error,
            message:"Ocurrio un error mientras se consultaban los datos"
        })
    }
}
```

**[⬆ Volver a índice](#indice)** 

---

## Opinión
La verdad que cuando descubrí sequelize muchos problemas se solucionaron. No solo porque te ayuda a evitar escribir en un lenguaje de consultas, acortando tiempos de trabajo, sino que también es muy tiene muchas funciones que te solucionan la vida. Una de ellas son los campos `createdAt` y `updatedAt`, que, por poco que parezca, te ayuda a no tener que pensar en ello, ya que lo hace automático.   
Pero lo que a mí más me interesa sobre sequelize es que te permite agregar o quitar campos de las tablas, sin necesidad de hacer todo ese trabajo manual y sin perder los datos. Con solo cambiar algunas propiedades de los Schemas ya es suficiente para que Sequelize entre en acción.  
Estoy bastante contento con esta tecnología y si quieren comenzar a trabajar con Sequelize yo te lo recomiendo.

**[⬆ Volver a índice](#indice)** 
