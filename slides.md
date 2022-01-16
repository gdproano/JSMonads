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
# persist drawings in exports and build
drawings:
  persist: false
---

# Mónadas en Javascrpit

David Proaño BP

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

- 📝 **Principios de la programacón funcional** 
- 📝 **Algebra** 
- 📝 **Functores** 
- 📝 **Mónadas** 
- 📝 **Bibliografía** 

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

# Principios de la programacón funcional

Se fundamenta fuertemente en las matemáticas, propone 

-  **Eliminar la mutabilidad de los estados** 
-  **Composición de funciones** 
-  **Transparencia referencial** 
-  **Funciones puras** 

---

# Inmutabilidad

Un objeto no puede ser mutado una vez creado.  Nos brinda la ventaja de no tener "efectos colaterales"
<br>
Para poder actualizar el estado de una propiedad se hace uso de funciones específicas las cuales retornan un objeto totalmente diferente.
<br>

Ejemplo:  Objetos memoizados.
<br>

- React.memo
- useCallback
- useReducer

---

# Composición de funciones

Creación de funciones simples hasta alcanzar las primitivas del lenguaje.
<br>
Debe cumplir con ser funciones de un solo propósito.
<br> 
Separación de estructuras de datos y funciones que operan sobre estas. 
<br>


```ts
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

# Función pura

- El valor a retornar solo depende de los argumentos de entrada
- No modifica nada que esté fuera de su interés
- Funciones de primer clase (pueden ser almacenadas en variables, ser pasadas como parámetro y devueltas desde funciones, sin tratamiento especial)
- El valor a retornar solo depende de los argumentos de entrada
- No modifica nada que esté fuera de su interés
- Ejemplos map, reduce y filter

```ts 
  const [value, setValue] = useState(0);
  setValue(2);
}
```

---

# Función pura
```ts
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

- Una expresión puede ser sustituida directamente por su valor sin que esto afecte a la ejecución del programa.
- Una función sólo retorna un valor y no produce cambios de estado, dependiendo de sus argumentos.
<br>
<br>

```ts
const add(a, b) => a+b
add(add(1,3), add(6,2))
add(4, add(6, 2))
add(4, 8)
//12
```

---

# Functores
- Contenedores con una característica especial: debe permitirnos transformar el valor interno en cualquier forma que nosotros queramos sin tener que dejar el contenedor.

```ts
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
El ejemplo fue el llamado functior identidad. Netamente ilustrativo para demostrar el patrón que usan los functores.
<br>
Los beneficios que nos proporcionan los functores es la abstracción de efectos de una computación pura.
<br>

---

# Ejemplo

Los arreglos siguen el patrón de los functores.
Permiten guardar múltiples valores en una misma estructura.
<br>
Array.map

```ts
const xbox = ['x'];
const to_uppercase = (str) => str.toUpperCase();

xbox.map(to_uppercase);

```

<b>Uso de los functores:</b>

- Manejo de errores
- Validar ausencia de valores
- Procesos asíncronos

---

# Mónadas
Una mónada es una especie de functor.
Función auxiliar que pueda colocar cualquier valor ordinario dentro de la unidad más simple de nuestra estructura. Esta función es conocida como "pure", otros nombres también incluyen "unit" y "of".

```ts
Array.of('¿en serio?');
// => Array [ "¿en serio?" ]

Array.of(42);
// => Array [ 42 ]

Array.of(null);
// => Array [ null ]

```




