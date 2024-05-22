# Módulo 1: Introducción a JavaScript

## 1.1 Historia y Uso de JavaScript

### Historia
- **1995**: Brendan Eich desarrolló JavaScript en 10 días en Netscape.
- **1996**: Microsoft lanzó JScript como respuesta.
- **1997**: ECMA (European Computer Manufacturers Association) estandarizó JavaScript como ECMAScript.
- **2015**: ECMAScript 2015 (ES6) fue una gran actualización, introduciendo muchas características modernas.

### Uso
- **Frontend**: Dinamiza páginas web, manipula el DOM, gestiona eventos.
- **Backend**: Con Node.js, permite crear servidores y aplicaciones backend.
- **Aplicaciones**: Desarrollo de aplicaciones móviles (React Native), aplicaciones de escritorio (Electron).

## 1.2 Sintaxis Básica: Variables, Operadores, Estructuras de Control

### Variables
En JavaScript, se pueden declarar variables usando `var`, `let` y `const`.

- `var`: Tiene alcance de función (function scope) y puede ser reasignada.
- `let`: Tiene alcance de bloque (block scope) y puede ser reasignada.
- `const`: Tiene alcance de bloque y no puede ser reasignada.

#### Ejemplos

```js
    var nombre = "Juan";
    let edad = 30;
    const pais = "España";
```

#### Ejercicios

- Declara una variable ciudad usando let y asigna tu ciudad actual. 
- Intenta reasignar un valor a una constante y observa el error.

### Operadores

JavaScript tiene varios operadores: aritméticos, de asignación, de comparación, lógicos, entre otros.

```js
    // Operadores aritméticos
    let suma = 5 + 3; // 8
    let resta = 5 - 3; // 2

    // Operadores de comparación
    let igual = (5 == "5"); // true
    let estrictamenteIgual = (5 === "5"); // false

    // Operadores lógicos
    let yLogico = (true && false); // false
    let oLogico = (true || false); // true
```

#### Ejercicios

- Realiza operaciones aritméticas con variables a y b.
- Compara dos variables usando == y === y explica la diferencia.
- Usa operadores lógicos para evaluar expresiones.

### Estructuras de Control

#### Condicionales

```js
    let edad = 20;

    if (edad >= 18) {
    console.log("Eres mayor de edad");
    } else {
    console.log("Eres menor de edad");
    }
```

#### Switch

```js
    let dia = 3;

    switch (dia) {
    case 1:
        console.log("Lunes");
        break;
    case 2:
        console.log("Martes");
        break;
    case 3:
        console.log("Miércoles");
        break;
    default:
        console.log("Día no válido");
    }

```
#### Bucles

```js
    // Bucle for
    for (let i = 0; i < 5; i++) {
    console.log(i);
    }

    // Bucle while
    let j = 0;
    while (j < 5) {
    console.log(j);
    j++;
    }
```

#### Ejercicios

- Escribe un condicional que verifique si un número es positivo, negativo o cero.
- Usa un switch para imprimir el nombre de un día de la semana basado en un número del 1 al 7.
- Crea un bucle for que imprima los números del 1 al 10.
- Crea un bucle while que imprima los números del 10 al 1.

## 1.3 Funciones y Scope

### Funciones

```js
    // Función declarada
    function saludar(nombre) {
    return "Hola, " + nombre;
    }

    // Función anónima
    const despedir = function(nombre) {
    return "Adiós, " + nombre;
    };

    // Función flecha
    const multiplicar = (a, b) => a * b;
```
#### Ejercicios

- Crea una función que sume dos números.
- Crea una función que convierta grados Celsius a Fahrenheit.
- Escribe una función flecha que devuelva el cuadrado de un número.

### Scope

El alcance determina la accesibilidad de las variables.

- Global Scope: Variables accesibles en cualquier parte del código.
- Function Scope: Variables accesibles solo dentro de una función.
- Block Scope: Variables accesibles solo dentro de un bloque (let, const).

```js
    let global = "Soy global";

    function prueba() {
    let funcion = "Soy de función";
    console.log(global); // Accede a variable global
    console.log(funcion); // Accede a variable de función
    }

    prueba();
    console.log(global); // Accede a variable global
    console.log(funcion); // Error, no accesible fuera de la función
```

#### Ejercicios

- Declara una variable global y úsala dentro de una función.
- Declara una variable dentro de una función y trata de usarla fuera de la función.
- Declara variables usando let y const dentro de un bloque y observa su accesibilidad fuera del bloque.

# 1.4 Arrays y Objetos

## Arrays
```js
    let frutas = ["Manzana", "Banana", "Naranja"];

    // Acceso a elementos
    console.log(frutas[0]); // Manzana

    // Métodos de Arrays
    frutas.push("Uva"); // Agrega un elemento al final
    frutas.pop(); // Elimina el último elemento
```

#### Ejercicios

- Crea un array de nombres y añade un nuevo nombre al final.
- Elimina el primer elemento de un array.
- Usa un bucle para imprimir todos los elementos de un array.

## Objetos

```js
    let persona = {
        nombre: "Carlos",
        edad: 28,
        saludar: function() {
            console.log("Hola, me llamo " + this.nombre);
        }
    };

    // Acceso a propiedades
    console.log(persona.nombre); // Carlos
    persona.saludar(); // Hola, me llamo Carlos

```
#### Ejercicios

- Crea un objeto coche con propiedades marca, modelo y año.
- Añade un método arrancar que imprima "El coche está en marcha".
- Accede y modifica una propiedad del objeto.

## Resumen y Proyecto Final

## Mini-Proyecto: Perfil de Usuario

## Descripción
## Crear un perfil de usuario usando JavaScript.

## Requisitos
- Crear un objeto usuario con propiedades como nombre, edad, email y un método mostrarPerfil que imprima los detalles del usuario.
- Crear un array amigos que contenga objetos amigo con propiedades nombre y email.
- Añadir, eliminar y listar amigos en la consola.

```js
    let usuario = {
    nombre: "Ana",
    edad: 25,
    email: "ana@example.com",
    mostrarPerfil: function() {
        console.log(`Nombre: ${this.nombre}, Edad: ${this.edad}, Email: ${this.email}`);
    }
    };

    let amigos = [
    { nombre: "Luis", email: "luis@example.com" },
    { nombre: "Maria", email: "maria@example.com" }
    ];

    // Funciones para gestionar amigos
    function agregarAmigo(nombre, email) {
    amigos.push({ nombre, email });
    }

    function eliminarAmigo(nombre) {
    amigos = amigos.filter(amigo => amigo.nombre !== nombre);
    }

    function listarAmigos() {
    amigos.forEach(amigo => console.log(`Nombre: ${amigo.nombre}, Email: ${amigo.email}`));
    }

    // Uso de las funciones
    usuario.mostrarPerfil();
    agregarAmigo("Carlos", "carlos@example.com");
    listarAmigos();
    eliminarAmigo("Luis");
    listarAmigos();
```