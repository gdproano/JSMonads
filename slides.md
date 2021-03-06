---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exporjs and build
drawings:
  persist: false
---

# M贸nadas en Javascrpit

David Proa帽o BP

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Continuar <carbon:arrow-right class="inline"/>
  </span>
</div>


<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

# Contenido

- 馃摑 **Principios de la programac贸n funcional** 
- 馃摑 **Functores** 
- 馃摑 **Algebra** 
- 馃摑 **M贸nadas** 
- 馃摑 **Bibliograf铆a** 

<br>
<br>

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/guide/syntax#embedded-styles
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
b {
  color: teal;
}
</style>

---

# Principios de la programac贸n funcional

Se fundamenta fuertemente en las matem谩ticas, propone 

-  **Eliminar la mutabilidad de los estados** 
-  **Composici贸n de funciones** 
-  **Transparencia referencial** 
-  **Funciones puras** 

---

# Inmutabilidad

- Un objeto no puede ser mutado una vez creado.  
- Nos brinda la ventaja de no tener "efectos colaterales"
- Para poder actualizar el estado de una propiedad se hace uso de funciones espec铆ficas las cuales retornan un objeto totalmente diferente.
<br>

Ejemplo:  Objetos memoizados.
<br>

- React.memo
- useCallback
- useReducer

---

# Composici贸n de funciones

Creaci贸n de funciones simples hasta alcanzar las primitivas del lenguaje.
<br>
Debe cumplir con ser funciones de un solo prop贸sito.
<br> 
Separaci贸n de estructuras de datos y funciones que operan sobre estas. 
<br>


```js
  const comp2 = (f, g) => ((...arg) => g(f(...arg)));
  const abs = comp2(x => x * x, Math.sqrt);
  abs(-4); // => 4

  const compose = function(...functions) {
  return functions.reduceRight(comp2);
  };

  // Las funciones se aplican de derecha a izquierda
  const f = compose(Math.floor, Math.log, Math.max)
  f(10, 20) // => 2

}
```

---

# Funci贸n pura

- El valor a retornar solo depende de los argumentos de entrada
- No modifica nada que est茅 fuera de su inter茅s
- Funciones de primer clase (pueden ser almacenadas en variables, ser pasadas como par谩metro y devueltas desde funciones, sin tratamiento especial)
- Ejemplos map, reduce y filter
<br>
<br>

```js 
  const [value, setValue] = useState(0);
  setValue(2);
}
```

---

# Funci贸n pura
```js
const list = [1, 2, 3, 4, 5];

const squared = list.map(x => x ** 2);
// => [1, 4, 9, 16, 25]

const product = list.reduce((acc, item) => acc * item, 1);
// => 120

const even = list.filter(x => x % 2 == 0);
// => [2, 4]
```
---

# Transparencia referencial

- Una expresi贸n puede ser sustituida directamente por su valor sin que esto afecte a la ejecuci贸n del programa.
- Una funci贸n s贸lo retorna un valor y no produce cambios de estado, dependiendo de sus argumentos.
<br>
<br>

```js
const add(a, b) => a+b
add(add(1,3), add(6,2))
add(4, add(6, 2))
add(4, 8)
//12
```

---

# Functores
- Contenedores con una caracter铆stica especial: debe permitirnos transformar el valor interno en cualquier forma que nosotros queramos sin tener que dejar el contenedor.
- Aplicaci贸n de funciones como otra propiedad de la estructura.
- Preservar la forma de la estructura


```js
function Box(data) {
  return {
    map(fn) {
      return Box(fn(data));
    }
  }
}

const xbox = Box('x');
const to_uppercase = (str) => str.toUpperCase();

xbox.map(to_uppercase).map(console.log);
// => X
// => Object { map: map() }
```

---
El ejemplo fue el llamado functior identidad. Netamente ilustrativo para demostrar el patr贸n que usan los functores.
<br>
Los beneficios que nos proporcionan los functores es la abstracci贸n de efectos de una computaci贸n pura.

---

# Algebra
 Dado $\ fx$ y $\ gx$ lo siguiente debe ser cierto.

 $\ value.map(fx).map(gx)$ y $\ value.map(arg => gx(fx(arg)))$ deben ser equivalentes

```js
function add_one(num) {
  return num + 1;
}

function times_two(num) {
  return num * 2;
}

[1].map(add_one).map(times_two);         // => [4]
[1].map(num => times_two(add_one(num))); // => [4]

```

Ventajas:

- Mejor el desempe帽o 
- Mejor legibilidad 

---

# Ejemplo

Los arreglos siguen el patr贸n de los functores.
<br>
Permiten guardar m煤ltiples valores en una misma estructura.
<br>
<br>
Array.map

```js
const xbox = ['x'];
const to_uppercase = (str) => str.toUpperCase();

xbox.map(to_uppercase);

```
<b>Uso de los functores:</b>

- Manejo de errores
- Validar ausencia de valores
- Procesos as铆ncronos

---

# Promises

<br>
Permiten guardar m煤ltiples valores en una misma estructura.
<br>
<br>
Array.map

```js
// Value
1;

// Box
Promise.resolve;

// Value on the box
Promise.resolve(1);

// Composition
Promise.resolve(1).then(add_one).then(times_two);        // => 4
Promise.resolve(1).then(num => times_two(add_one(num))); // => 4

```
---

# En otra estructura

```js
const Obj = {
  map(fn, ob) {
    let result = {};
    for (let [key, value] of Object.entries(ob)) {
      result[key] = fn(value);
    }

    return result;
  }
};

Obj.map(times_two, Obj.map(add_one, {some: 1, prop: 2})); // => {some: 4, prop: 6}
Obj.map(num => times_two(add_one(num)), {some: 1, prop: 2}); // => {some: 4, prop: 6}

```
---

# M贸nadas
Una m贸nada es una especie de functor que puede aplanarse.
Funci贸n auxiliar que pueda colocar cualquier valor ordinario dentro de la unidad m谩s simple de nuestra estructura. Esta funci贸n es conocida como "pure", otros nombres tambi茅n incluyen "unit" y "of".

```js
Array.of('驴en serio?');
// => Array [ "驴en serio?" ]

Array.of(42);
// => Array [ 42 ]

Array.of(null);
// => Array [ null ]

```

---

驴Qu茅 pasar铆a si queremos agregar un efecto?

```js
const num = Box(41);
const action = (num) => Box(num + 1);

const res = num.map(action);

// Box(Box(42))

```

---
Permiten fusionar capas de estructuras anidadas innecesarias.
驴C贸mo? 
<br>

```js
function Box(data) {
  return {
    map(fn) {
      return Box(fn(data));
    },
    join() {
      return data;
    }
  }
}

const num = Box(41);
const action = (num) => Box(num + 1);

const res = num.map(action).join();

res.map(console.log);

```

---
Los arreglos son functores, en realidad los usamos a diario pero tambi茅n son m贸nadas y los podemos usar de muchas maneras.

```js
[[41], [42]].flat();
// => Array [ 41, 42 ]
```
<br>
<br>

---

# M贸nadas en secuencia

Resulta que esta combinaci贸n de map/join es tan com煤n que hay un m茅todo que combina las caracter铆sticas de esos dos. Este tambi茅n tiene varios nombres: "chain", "flatMap", "bind", ">>=" (en haskell). Los arreglos lo llaman flatMap.

```js
const split = str => str.split('/');

['some/stuff', 'another/thing'].flatMap(split);

// monad.flatMap(action)
//   .map(another)
//   .map(cool)
//   .flatMap(getItNow);
```

---
# Conclusiones

- Este patr贸n nos permite enfocarnos en un problema a la vez. La funci贸n map se encarga de obtener los datos necesarios y en el callback nos podemos enfocar en c贸mo procesarlos.

- Reutilizaci贸n. Este estilo de programaci贸n promueve el uso y creaci贸n de funciones de generales que s贸lo se encargan de una tarea, en muchos casos estas pueden ser compartidas incluso entre proyectos.

- Extensi贸n a trav茅s de la composici贸n. Hay gente que tiene sentimientos encontrados en este caso, especialmente si hablamos de aplicarlo a los arreglos. Pero lo que quiero decir es que los functors promueven el uso de cadenas de funciones para implementar un procedimiento.

