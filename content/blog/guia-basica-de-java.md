---
title: Guía básica de Java
date: "2023-04-02"
description: "En este artículo aprenderás lo que necesitas saber para comenzar a trabajar con este lenguaje de programación."
---

En este artículo aprenderás lo que necesitas saber para comenzar a trabajar con este lenguaje de programación. Mi intención no es hacer una explicación detallada sobre el mismo, sino explicar el funcionamiento de Java de una forma concisa y práctica.

<div id='indice' />

## Indice
* [Que es Java?](#queesjava)
* [Como funciona Java?](#como-funciona-java)
* [Clases](#clases)
    * [Nomenclatuara para definir clases](#nomenclaturaparadefinirclases)
    * [Importante](#importante)
    * [Constructor](#constuctor)
    * [Estructura](#estructuradelasclases)
* [Paquetes](#paquetes)
    * [Sub-Paquetes](#subpaquetes)
* [Variables](#variables)
    * [Variables primitivas](#variablesprimitivas)
    * [Como declarar variables](#comodeclararvariables)
* [Constantes](#constantes)
* [Arrays](#arrays)
* [Operadores Logicos](#operadoreslogicos)
* [Math](#clasemath)

---

## Que es Java?
Antes de adentrarnos en la sintaxis de Java, es importante conocer un poco sobre el mismo y como funciona.  

Java es un lenguaje de programación de alto nivel. Su creador *James Gosling* lo define en base a sus 
características: sencillo, orientado a objetos, distribuido, interpretado, robusto, seguro, 
independiente de las arquitecturas, portable, eficaz, multitarea y dinámico.

Se utiliza para crear software compatible con una gran diversidad de sistemas operativos. Este lenguaje tiene la particularidad de ser compilado e interpretado al mismo tiempo; esto significa que es un lenguaje simplificado que convierte automáticamente el código en instrucciones de máquina.


**[⬆ Volver a índice](#indice)**   

---

<div id='como-funciona-java' />

## Como funciona Java?

En el lenguaje de programación Java, todo el código fuente se escribe en archivos de texto plano con la extensión *.java*. Estos archivos fuente son entonces compilados por una herramienta de la plataforma denominada compilador javac guardando el resultado en archivos con el mismo nombre, pero con 
extensión *.class*. Un archivo *.class* no contiene código nativo 
para el procesador de la plataforma base, sino que contiene un 
tipo de código denominado **bytecode** (lenguaje binario 
intermedio). Los bytecode se pueden considerar el lenguaje 
máquina de la JVM, debido a que la JVM es la encargada de 
traducir los bytecode en el código máquina del ordenador

Vamos a verlo en forma gráfica para entenderlo mejor:

![funcionamiento](https://res.cloudinary.com/dyrpgj8od/image/upload/v1681389920/Blog/funcionamiento_wcpc1r.png)

**[⬆ Volver a índice](#indice)**   

---

## Clases

En Java el código debe ser programado dentro de unos contenedores denominados clases. Si no 
se cumple esta premisa, el compilador del lenguaje marcará error. El lenguaje utiliza estos contenedores debido a que se rige por el Paradigma Orientado a Objetos.

Para poder continuar con la explicacion vamos a poner de ejemplo el siguiente caso:

> En San Salvador de Jujuy, se incentiva el uso de bicicletas a través de corredores, tales como los presentes en el Parque Lineal, algunas calles de la ciudad y otros a proyectarse en el futuro. Se espera con ello promover el cuidado de la salud a través del ejercicio y por otro lado que circulen menos vehículos por la ciudad. Dentro del plan de incentivos del uso de bicicletas, estas son prestadas en diferentes puestos colocados de manera estratégica. Se necesita una herramienta que sea capaz de realizar el seguimiento de la gestión de préstamos a personas.

Las clases son contenedores que indican el objeto que representan, con qu datos trabajan y que acciones pueden realizar.

Ahora, teniendo esto en cuenta, y trabajando en base al ejemplo antes mencionado, podriamos definir 3 clases para realizar el proyecto.

``` java
public class Bicicleta(){

}
```

``` java
public class PuestoPrestamo(){

}
```

``` java
public class Persona(){

}
```

Ahora voy a describir las partes de las declaraciones anteriores:

* Las llaves indican el comienzo y el final de la clase
* la palabra *public* acompaña a la clase principal del programa. Dentro de esta clase pueden declararse otras clases.

### **Nomenclatura para definir clases**

Es importante seguir ciertas reglas a la hora de declarar las clases. Estas son:
* Los nombres de las clases deben ser sustantivos en Singular.
* La primera letra del nombre de la Clase debe ser escrita en mayúscula y el resto en minusculas.
* En caso de conformar el nombre de la clase con varias palabras se indica la distinción de cada una de ellas con letra en mayúscula.

### **Importante**
A la hora de declarar una clase en un archivo nuevo, de formato *.java*, es importante que la clase principal tenga el mismo nombre que el archivo, porque sino ocurrira un error.

Solo puede existir una clase definida como *public* en el archivo *.java* y es esta.

### **Constuctor**

El metodo constructor sirve para darle valores dinamicos a los atributos de una clase. Con esto podemos darle valores predeterminados a la clase o bien los indicados en el constructor.

Se declara de la siguiente manera

```java
public class Persona {
    String nombre;
    int edad;

    public Persona(String _nombre, int _edad){
        // Los parametros se recomienda escribirse con _ antes del nombre del atributo a asignar, como se ve. 
        
        nombre = _nombre;
        edad = _edad;
    }
}
```

Para pasar estos parametros lo hacemos con:

```java
Persona persona1 = new Persona("Juan", 32)
```
> Cabe recalcar que no es obligatorio, solo se debe definir cuando se necesita.

### **Estructura de las clases**

Las clases estan conformadas por *atributos* y *metodos*.

Los **atributos** respresentan las caracteristicas del objeto. Serian las variables del objeto. Estas pueden ser definidas de manera estatica o mediante un constructor. 

``` java
public class Persona(){
    static String nombre = "Juan";
    static Integer edad = 22;
}
```

Los metodos representan la funcionalidad del objeto. Son las acciones que puede realizar el objeto y, normalmente, utiliza los valores de sus atributos.
``` java
public class Persona(){
    static String nombre = "Juan";
    static Integer edad = 22;

    public static void main(){
        .....
        .....
        example();
    }

    static void example(){
        System.out.printLn("El nombre es: " + nombre);
        System.out.printLn("La edad es: " + edad);
    }
}
```

> El metodo *main* es el metodo que se ejecuta automaticamente cuando el programa comienza a funcionar. Este siempre lleva el nombre *main*, acompañado de la palabra *public*.


**[⬆ Volver a índice](#indice)**   

---

## Paquetes

Los paquetes permiten organizar los archivos *.java* (y en consecuencia las clases) en categorías que representen características comunes entre las clases. No pueden existir dentro de un paquete clases con el mismo nombre, pero si es posible que existan clases con el mismo nombre en diferentes paquetes.

Al organizar archivos .java en paquetes se proporciona un mecanismo sencillo para acceder a clases almacenadas en otros paquetes (usando la sentencia *import*).

> Los archivos *.java*, osea las clases, dentro de un mismo paquete pueden ser instanciados entre si, sin utilizar la sentencia *import*.

### **Sub Paquetes**

Un paquete puede contener a uno o varios paquetes (denominados sub-paquetes). Para indicar la jerarquía de paquetes a la que pertenece un archivo .java se establece un separador entre los 
nombres de los paquetes de la jerarquía. Este separador es el “.”

![paquetes](https://res.cloudinary.com/dyrpgj8od/image/upload/v1681389920/Blog/paquetes_olqaic.png)

**[⬆ Volver a índice](#indice)**   

---

## Variables

Tenemos las variables de tipo **Entero**, donde podemos encontrar

| Nombre | Tamaño | Rango 
|--------|--------|-------
| long   | 64     | Numero muy grande
| int    | 32     | -2,147,483,648 a 2,147,483,647
| short  | 16     | -32,768 a 32,767
| byte   | 8      | -128 a 127

Variables de tipo **Reales**

| Nombre | Tamaño | Rango 
|--------|--------|-------
| double | 64     | 4.9e-324 a 1.8e+308
| float  | 32     | 1.4e-045 a 3.4e+038

Por ultimo tenemos a:  
**Boolean** que solo acepta *True* o *False*

### **Variables primitivas**

Recién vimos unos tipos de variables, las cuales son no primitivas.  
Ahora vamos a ver cuáles son las primitivas, y una diferencia que podemos encontrar en estas es que las Primitivas aceptan el ***null*** mientras que las no primitivas no lo reconocen.
**Integer**: Número entero  
**String**: Cadena de texto/conjunto de caracteres(*Char*) 

> Algo muy importante de los datos primitivos es que estos cuentan con Métodos, los cuales son esenciales para trabajar sobre los mismos.

### **Como declarar variables**
Esto es bastante parecido a pascal, pero lo diferencia es que hacemos todo en el mismo código.

```java
String nombre1 = "Palabra";
byte nombre2 = 4;    
```

> Aqui declaramos una variable "nombre1" de tipo *String* y una variable "nombre2" de tipo *byte*

**[⬆ Volver a índice](#indice)**   

---

## Constantes
Estas se declaran como las variables, a diferencia que estas tienen una palabra representativa al inicio

```java
final String nombre1 = "Palabra";    
```

No solo se diferencian en la forma en que se declaran, sino que las constantes, como su nombre lo indica, no pueden ser modificadas. Una vez que se declaran, con cierto valor, nunca se modifican.

> Normalmente se utilizan las variables para definir variables que rara vez cambian y se utilizan en varios lugares.

**[⬆ Volver a índice](#indice)**   

---

## Arrays

Los arrays o arreglos son una coleccion de elementos. Pueden contener cadenas de texto, enteros, otros arrays, entre otros.

Los arrays en Java se declaran al inicio del programa, con su respectivo tamaño y el tipo de dato que acepta.

**Tipo_de_variable[] Nombre_del_array = ***new*** Tipo_de_variable[ Dimension ]**

En java quedaria de la siguiente manera:

```java
int[] edades = new int[5]   //Array de tipo int con tamaño 5 [0..4]
```
> Comienza desde el 0, no es como en pascal que puede ser definido. Automaticamente es ``0..n-1``

Algo a tener en cuenta, es que en java, a diferencia de pascal, podemos elegir de forma dinamica, usando JOptionPane, el numero de elementos del array, ya que en java podemos declarar las variables en cualquier lugar. Un ejemplo para hacer esto es el siguiente:

```java
int elementos;
elementos = Integer.parseInt(JOptionPane.showInputDialog("Digite la cantidad de elementos del array:"))

int[] edades = new int[elementos]  

//Dependiendo de los datos ingresados por el usuario, ese sera el tamaño del array
```

También podemos declarar arrays con valores ya agregados. Así:

```java
String[] nombres = { "Juan","Hernan","Luis","Miguel" }  
//Se crea un array de tipo String y de dimensión 4 [0..3] con los valores ya declarados.
```

**[⬆ Volver a índice](#indice)**   

---

## MATRICES
Las matrices son arrays bidimensionales, ya que necesita de dos parámetros ``arreglo[{*Filas*}][{*Columnas*}]``.  
Estas podemos declararlas de dos formas. Una es de forma manual o estática y la otra de forma dinámica, osea, proporcionada por el usuario.

```java
//Matriz manual de 2 Filas y 3 Columnas
int matrizM[][] = {{1,2,3},{4,5,6}}; 

//Matriz Dinámica con filas y columnas proporcionadas
int matrizD[][], nFilas, nColum;

nFilas = Integer.parseInt(JOptionPane.showInputDialog("Ingrese las filas"));
nColum = Integer.parseInt(JOptionPane.showInputDialog("Ingrese las columnas"));

matrizD = new int[nFilas][nColum];
```

> Para leer estas utilizamos el operador FOR 2 veces, para leer las filas a la vez que leemos las columnas. Lo mismo para agregar elementos, claro.   

**[⬆ Volver a índice](#indice)**   

---

## Operadores logicos
Con los operadores logicos podremos efectuar operaciones comparativas. Estas son esenciales para asegurar el correcto funcionamiento del programa.

No son muchos, pero con cada uno podemos hacer varios tipos de comparaciones.

![Funciones de Math](https://res.cloudinary.com/dyrpgj8od/image/upload/v1680112745/Blog/Operadores_comparacion_rncsaa.png)

**[⬆ Volver a índice](#indice)**   

---

## CLASE MATH

Con la clase Math podemos realizar cualquier tipo de operación matemática. Tiene muchas funciones, pero solo vamos a ver las mas utilizadas o mas básicas.

![Funciones de Math](https://res.cloudinary.com/dyrpgj8od/image/upload/v1680112746/Blog/clase_Math_tfhuru.png)

**[⬆ Volver a índice](#indice)**   