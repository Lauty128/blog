---
title: Como solucionar problema de react router dom en producción
date: "2023-04-25"
description: "Si estas aquí, seguramente, es porque acabas de subir tu aplicación de react junto a react-router-dom, y descubriste que al refrescar la página, siendo esta distinta de la ruta principal, se te rompe la aplicación y devuelve un error 404."
tags: ["Programación"]
---

Si estas aquí, seguramente, es porque acabas de subir tu aplicación de react junto a react-router-dom, y descubriste que al refrescar la página, siendo esta distinta de la ruta principal, se te rompe la aplicación y devuelve un error 404.

Por eso en este artículo voy a explicarte la razón de este problema y dos formas de solucionarlo.

<a id="indice"></a>

## Indice

* [Porque ocurre este problema?](#porque-ocurre-este-problema)
* [Porque al usar Link no ocurre un error?](#porque-al-redireccionar-con-un-componente-link-no-ocurre-un-error)
* [Como se solucionaria?](#como-se-soluciona)
* [Solucion con NodeJs](#solución-con-nodejs)
    * [Crear servidor](#crear-y-configurar-servidor)
    * [Configurar rutas](#configurar-rutas)
    * [Deploy](#deploy)
* [Solucion con Netlify](#solución-con-netlify)

---

<a id="porque-ocurre-este-problema"></a>

## Porque ocurre este problema?

Para comprender este problema, primero debemos entender como funciona un servidor web. 

> Yo no cuento con conocimientos avanzados en este tema, pero es muy importante conocer sobre esto, ya que nos ayudara a la hora de tener en cuenta sobre donde y como hacer el deploy de la aplicación que se está desarrollando.

Cuando se accede a un sitio web en su ruta principal, osea `https://domain.com`, el servidor buscara el archivo index.html para servirlo en la página. Si se quiere extender la url para acceder a la sección *sobre mi*, haríamos lo siguiente:

```
https://domain.com/sobremi
```

Esto es lo que hay que comprender.   
Cuando indicamos esto, el servidor, si nadie le indica que debe hacer con esa url, va a buscar el archivo `sobremi.html`, el cual no existe. Y es por eso que al refrescar la página, el servidor devolverá un error 404.

**[⬆ Volver a índice](#indice)**   

---
<a id="porque-al-redireccionar-con-un-componente-link-no-ocurre-un-erro"></a>

## Porque al redireccionar con un componente Link no ocurre un error?

Cuando se usa el componente `Link`, que proporciona react-router-dom, para redireccionar a la página a `/sobremi` funciona correctamente.   
Esto es porque, cuando usamos `Link`, en vez de la etiqueta ``, se trabaja directamente sobre el cliente, no se solicita ningún dato al servidor, por lo que este no interferirá en las operaciones que se realicen.

Esto react-router-dom lo hace para optimizar la carga del sitio mediante procesos de su estructura interna. Esto nos beneficia cuando el usuario interactúa con la página desde la ruta principal, pero cuando refresque el navegador, estando en una ruta distinta a la principal, se romperá el sitio, ya que ahí si va a interferir el servidor.

**[⬆ Volver a índice](#indice)**   

---
<a id="indice"></a>

## Como se soluciona?

La solución a este problema es más fácil de lo que parece. Lo que se debe hacer es indicarle al servidor lo que tiene que hacer, cuando ingresamos en una ruta distinta a la principal.

En vez de que, al entrar en `/sobremi`, busque el archivo `sobremi.html`, se debe indicar que, independientemente de la ruta que sea, siempre se sirva al archivo `index.html`.

Para hacer esto tenemos dos formas: 

* Crear nuestro propio servidor y configurarlo.
* Indicar al servidor, mediante un archivo, que siempre acceda al *index.html*   

**[⬆ Volver a índice](#indice)**   

---
<a id="solucion-con-nodejs"></a>

## Solución con Nodejs

La primera solución es la de crear nuestro propio servidor con Nodejs, utilizando Express.   
Si no tienes conocimientos sobre Nodejs no te preocupes, voy a dejar toda la información posible para que puedas configurar tu propio servidor. Sino también tienes la otra forma de hacerlo, que no requiere crear un servidor, solo basta con configurar [Netlify](#solucion-con-netlify).

Voy a explicar desde 0 como crear el servidor, por lo que si ya sabes estos pasos los puedes saltear e ir a la [configuración de las rutas](#configurar-rutas).   
Debes tener intalado [Nodejs](https://nodejs.org/es) en tu sistema operativo para continuar.

<a id="crear-y-configurar-servidor"></a>

### **Crear servidor**

Lo primero que vamos a hacer es iniciar un proyecto de Nodejs.   
Para eso vamos a la terminal y nos ubicamos en la carpeta que deseamos crear el proyecto. Luego escribimos el siguiente comando:

    npm init -y

Luego procedemos a instalar express para configurar nuestro proyecto: 

    npm i express

Una vez que tenemos todo instalado vamos a configurar nuestro archivo `package.json`. Yo se los voy a servir listo para copiar y pegar, aunque pueden configurarlo según sus gustos.

``` javascript
{
"name": "routes",
"version": "1.0.0",
"description": "",
"main": "index.js",
"type": "module",
"scripts": {
    "start": "node index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
},
"author": "",
"license": "ISC",
"dependencies": {
    "express": "^4.18.2"
}
}
```
<a id="configurar-rutas"></a>

### **Configurar rutas**

Con todo esto ya podemos comenzar a configurar el servidor. 
Antes de hacer esto, primero debes tener tu proyecto de react terminado y debe tener hecha la *build* (Ejecutar el comando `npm run build`).   
Si el proyecto está hecho con ViteJs, tendrás una carpeta `dist`. Lo que debes hacer con esa carpeta es copiarla en la carpeta del proyecto de NodeJs, para así poder utilizarla.

Te debería quedar una estructura del proyecto así:

| - node_modules   
| - dist   
| - index.js   
| - package.json   
| - package-lock.json   

Ahora debemos configurar el archivo index.js, el cual debes crear tú (No lo había comentado antes).

``` javascript
//---- Dependencies
    import express from "express";
    import path from "path";
    import * as url from 'url';

// Initialize express
    const app = express();
    const __dirname =  url.fileURLToPath(new URL('.', import.meta.url));

// Settings
    app.set("port", process.env.PORT || 3000);
    app.use(express.static(path.join(__dirname,'dist')));

// Routes
    app.get("/*", (req, res) => {
        res.sendFile(path.join(__dirname, "dist/index.html"));
    });

// Listening the Server
    app.listen(app.get("port"), ()=>{
        console.log("Server on port", app.get("port"));
    });
```

En vez de usar, `app.get('/*')`, pueden marcar de forma manual solo las rutas que quieran, y dejar otras sin marcar, con el fin de manejar los errores.

Por ejemplo, suponiendo que se tienen 4 rutas:

* '/'
* '/sobremi'
* '/articulos'
* '/dashboard'

Si quieren que al refrescar la página en la ruta `/dashboard` no se quede guardada, pueden hacer esto

``` javascript
// Routes
    app.get("/", (req, res) => {
        res.sendFile(path.join(__dirname, "dist/index.html"));
    });

    app.get("/sobremi", (req, res) => {
        res.sendFile(path.join(__dirname, "dist/index.html"));
    });

    app.get("/articulos", (req, res) => {
        res.sendFile(path.join(__dirname, "dist/index.html"));
    });
```

> No se indica nada sobre la ruta `/dashboard`, por lo que el servidor devolvera un error

<a id="deploy"></a>

### **Deploy**

Lo que debes hacer a continuación es subir el servidor a un hosting que acepte a NodeJs. Hay muchas opciones, entre las gratuitas están:

* [Railway](https://railway.app/)
* [Vercel](https://vercel.com/login?next=%2Flauty128)
* [Render](https://render.com/)
* [Netlify](https://www.netlify.com/)

Algo muy importante que se debe hacer antes de subir el servidor es deshabilitar la carpeta node_modules para subir al repositorio. Eso lo hacemos creando un archivo llamado `.gitignore`. Dentro de este archivo solo debemos indicar la ruta de la carpeta que queremos ignorar:

    node_modules

Con eso ya ignoramos la carpeta `node_modules` para subir a nuestro repositorio.

> Para hacer el deploy de nuestro servidor nodejs no es muy complicado. Puedes ver un video de 5 minutos para ver como hacer el deploy. El servidor está configurado para subirse sin problemas.

**[⬆ Volver a índice](#indice)**   

---

<a id="solucion-con-netlify"></a>

## Solución con Netlify

Esta solución la encontré en un articulo que encontré. Si quieres mirarlo puede hacerlo dando click [aca](https://answers.netlify.com/t/support-guide-i-ve-deployed-my-site-but-i-still-see-page-not-found/125/12?utm_source=404page&utm_campaign=community_tracking).

Esta forma es tan fácil como crear un archivo llamado `netlify.toml` con el siguiente contendió dentro:

```
[build]
    command = "npm run build"
    publish = "/build"
    base = "/"

[[redirects]]
    from = "/*"
    to = "/index.html"
    status = 200
```

Esto lo que hará es avisarle al servidor que, cada vez que se acceda a una ruta, sin importar cual sea, siempre va a pasar por el `index.html`.

Como verán, esta forma es muy sencilla, aunque tiene sus limitaciones.   
Si no tienen un proyecto tan grande, pueden utilizar esta solución sin problemas. Pero como dije, todo depende de los requerimientos del proyecto.  

**[⬆ Volver a índice](#indice)**   

---


Muchas gracias por leer el artículos. 😁Un saludo!!