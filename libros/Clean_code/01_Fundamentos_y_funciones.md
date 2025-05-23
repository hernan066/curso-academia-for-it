# Clean Code: Resumen de los Capítulos 1 al 3

📃 Esto es un resumen de los conceptos claves, del capitulo 1 al 3.

## Capítulo 1: Clean Code

### There Will Be Code
- El desarrollo de software siempre implicará escribir código, no solo diseñar diagramas o documentos.

### Bad Code
- El código desordenado es más difícil de entender y mantener.
- La deuda técnica se acumula y ralentiza el desarrollo futuro.

### The Total Cost of Owning a Mess
- El "costo total" del código malo no solo incluye tiempo de desarrollo, sino mantenimiento, errores y rediseño.

### The Grand Redesign in the Sky
- Eventualmente los equipos intentan reescribir sistemas caóticos, pero sin disciplina recaen en los mismos errores.

### What Is Clean Code?
- Legible, simple, sin duplicaciones, expresivo y con un diseño bien organizado.
- Ejemplo en TypeScript:
```ts
function calculateTotalPrice(products: Product[]): number {
  return products.reduce((sum, p) => sum + p.price, 0);
}
```

### We Are Authors
- El código se lee muchas más veces de las que se escribe. Escribirlo para humanos, no para máquinas.

### The Boy Scout Rule
- "Deja el código mejor de lo que lo encontraste". Refactoriza constantemente.

---

## Capítulo 2: Nombres con Significado (Meaningful Names)

### Use Intention-Revealing Names
- Un buen nombre deja claro qué hace algo.
```ts
// Malo
let d: number;
// Bueno
let elapsedTimeInDays: number;
```

### Avoid Disinformation
- No uses nombres que induzcan a error, como `l`, `O`, `I`.

### Make Meaningful Distinctions
- `getData()` y `getInfo()` no se distinguen; usa nombres únicos con sentido.

### Use Pronounceable & Searchable Names
```ts
// Malo
let qzmtx: number;
// Bueno
let userScore: number;
```

### Avoid Encodings
- No usar notaciones como `m_`, `sz`, `iHungarian`, etc.

### Class & Method Names
- Clases: sustantivos (`OrderService`)
- Métodos: verbos (`calculateTotal`)

### Don’t Be Cute / Don’t Pun
- Usa nombres directos, no chistes ni juegos de palabras.

### Add Meaningful Context
```ts
// En lugar de:
let state = "CA";
// Mejor:
address.state = "CA";
```

### Don’t Add Gratuitous Context
- Evita prefijos innecesarios como `GSDAccountAddress`.

---

## Capítulo 3: Funciones

### Small!
- Las funciones deben ser pequeñas (2-5 líneas idealmente).
- Evitar funciones de más de 20 líneas.

### Do One Thing
- Cada función debe hacer **una sola cosa**. Si puedes dividirla en subfunciones, probablemente hace más de una.

### One Level of Abstraction per Function
- Evitar mezclar niveles de abstracción.
```ts
// Nivel alto:
function generateReport() {
  const users = fetchUsers(); // Nivel medio
  const html = renderToHtml(users); // Nivel bajo
  return html;
}
```

### Use Descriptive Names
- El nombre debe dejar claro el propósito de la función.

### Function Arguments
- Ideal: 0 a 2 argumentos.
- Evitar más de 3 (usar objeto si es necesario).

### Have No Side Effects
- Una función que "parece" leer no debería modificar estado.

### Command Query Separation
- Una función debe **consultar** o **modificar**, pero no ambas.

### Prefer Exceptions to Returning Error Codes
- Usar `throw` en vez de retornar códigos numéricos de error.

### Don’t Repeat Yourself (DRY)
- Evitar duplicaciones, extraer funciones comunes.

### How Do You Write Functions Like This?
- Mediante práctica, refactorización, revisiones y feedback.




