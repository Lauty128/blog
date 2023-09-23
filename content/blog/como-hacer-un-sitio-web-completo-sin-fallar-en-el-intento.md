---
title: Como hacer un sitio web completo sin fallar en el intento
date: "2023-03-23"
description: "Como hacer un sitio web completo, con backend y fronted, de una forma ordenada y planificada, con el fin de crear un proyecto limpio y ordenado."
tags: ["Programación"]
---

Ya cuento con un poco de experiencia como para poder ayudar a otras personas a no estancarse a la hora de trabajar en un proyecto. Por eso este blog se trata sobre como hacer un sitio web completo, con backend y fronted, de una forma ordenada y planificada, con el fin de crear un proyecto limpio y ordenado.
 
Desde mi experiencia y punto de vista puedo aconsejar sobre proyectos pequeños, como lo es este blog, ya que nunca trabaje en un proyecto tan grande. Pero puedo ayudar a personas que trabajan o quieren trabajar en un proyecto personal para adquirir experiencia.

La idea de este artículo es ayudar a estructurar un proyecto, desde sus raíces, para poder agregar o modificar funcionalidades en el mismo. Todo esto es para que al largo plazo no existan dificultades cuando vayamos a tocar el código. Esto lo digo porque a mí me paso con este blog, ya que no lo puedo modificar o agregar nuevas funcionalidades debido a que, en su momento, no tenía los conocimientos que tengo ahora, por lo que este proyecto tiene una base bastante floja como para agregar más funciones al mismo, por lo que debo reestructurar todo el proyecto sobre bases más firmes. Y créanme que es un trabajo bastante largo y estresante.

## Contexto

Vamos a ponernos en contexto tomando como ejemplo este blog.  

![imagen sobre proyecto](https://res.cloudinary.com/dyrpgj8od/image/upload/v1679609442/Blog/project_eveqps.png)  

* La app está construida con Nextjs
* Para la API usé NodeJS junto con Express
* De base de datos usé MongoDB
* Con cloudinary almaceno y solicito las imágenes

No quiero dar tantas vueltas para enseñarles toda la estructura del blog, ya que después vamos a adentrarnos un poco.  
Así que como ven en la imagen así es la estructura del proyecto y ahora les iré comentando que error tuve en cada parte del mismo.

## API
Con la API tuve un error gravísimo, del que me di cuenta demasiado tarde.  
Cuando ustedes van a trabajar en un proyecto que requiere de una API propia, deben trabajar a fondo sobre la misma para encontrar el mayor número de variantes que puede llegar a tener. Como yo no hice ese trabajo, pague las consecuencias poco después.   
Al dejar la API a medias, luego, mientras trabajaba con la APP, iba descubriendo errores o funciones que le hacían falta a la API para que trabaje de forma óptima, por lo que perdía demasiado tiempo trabajando con la API y la APP a la vez, y eso me fastidiaba demasiado, a la vez que me hacía consumir energía de más.   
Deben fijarse, mediante aplicaciones como [Postman](https://www.postman.com/), que su API funcione adecuadamente y devuelva una respuesta que pueda ser utilizada para notificar al usuario sobre el funcionamiento de las aplicación.   
Es por eso que recomiendo anotar, ya sea en un cuadernillo o en una aplicación, todos los acontecimientos que pueden llegar a suceder y como responder ante ellos.  

## MONGODB
No fueron tanto los problemas que tuve con la base de datos, pero es verdad que con un solo error que tengas en la planificación de la misma puede traerte problemas muy graves. Por suerte yo no contaba con tanta información en esta, por lo que no fue muy difícil acomodarla y lo más importante es que no demando de mucho tiempo.  
Cuando vayan a estructurar la base de datos, analicen todas las variantes posibles y plantéense la pregunta de <<¿Porque debería hacerlo así?>> o <<¿Sería mejor de esta otra forma?>> <<¿Porque?>>  
Y si están pensando en una función que vas a usar a largo plazo también debes tenerla en cuenta a la hora de estructurar la base de datos. Puede que tengas 1 o 2 propiedades de más que no utilices en el proyecto, pero cuando de verdad las precises van a estar ahí, por lo que no vas a tener que trabajar de más.

## APP
La app, aunque no parezca, más si es un proyecto de varias páginas, puede ser la más problemática. Puede ser por una estructura de carpetas errónea o una metodología de clases confusa, etc. Existen muchas variantes para hacer que tu proyecto sea confuso una vez que ya se ha escrito bastante código.  
Aquí quiero dejarles unas recomendaciones que me ayudaron a comprender un poco más la importancia de una buena estructura de carpetas y archivos. No importa si tienes más de 100 componentes en tu proyecto, esto luego se va a renderizar y no lo vas a ver. Lo importante es que tengas un código legible y que se pueda entender con facilidad.  
Algunos tips para mejorar el código:  
* **Metodología BEM** (es la que yo utilizo) para organizar las clases de tus elementos
* **Sass** para escribir CSS de una forma más cómoda. La verdad que te ayuda muchísimo, tiene unas funciones que te vuelan la cabeza como los Mixins, anidamientos, condicionales, entre otros
* **Arquitectura**: Es una muy buena práctica implementar una arquitectura de carpetas ya creada o crear una propia, siempre y cuando te sea cómoda y, a su vez, funcione bien.
* **Documentación**: Es bastante importante para explicar el funcionamiento de la aplicación y su flujo de datos.
* **Typescript**: Es una opción bastante util, ya que te ayuda a generar código más claro, confiable y sostenible. Actualmente cuento con pocos conocimientos sobre este lenguaje, pero por lo poco que se puedo decir que es de gran ayuda implementarlo de forma correcta

Muchas gracias por dedicar unos minutos de tu tiempo a leer este artículo. Ya saben que, como siempre digo, cualquier sugerencia que tengas estoy contento de recibirlas. Lo que comparto aquí es mi conocimiento, no quiero afirmar que esta es la forma correcta de hacerlo, pero es lo que yo sé basándome en mi experiencia   

Pueden comunicarse conmigo desde la seccion [contacto](/contacto)