# Clean Code: Resumen de los Capítulos 4 al 6

📃 Esto es un resumen de los conceptos claves, del capitulo 4 al 6.

## Capítulo 4: Comentarios

### Comentarios No Compensan Código Malo
- Si el código necesita explicación, probablemente necesita refactorización.
- El mejor comentario es un código claro.

### Buenos Comentarios
- **Legales**: necesarios por contrato o licencia.
- **Informativos**: explican decisiones técnicas difíciles.
- **Explicación de Intención**:
```ts
// Usamos búsqueda binaria para optimizar la búsqueda
```
- **Aclaraciones**: cuando el código no es evidente.
- **Advertencias**: sobre efectos secundarios o límites.
- **TODOs**: indicar trabajo pendiente.
- **Amplificación**: información adicional que no es obvia.
- **Javadocs en APIs públicas**: para consumidores externos.

### Malos Comentarios
- **Ininteligibles** o **murmullos**.
- **Redundantes**: describen lo obvio.
```ts
// Suma a + b y retorna el resultado
function sum(a: number, b: number) {
  return a + b;
}
```
- **Engañosos**: no coinciden con el comportamiento real.
- **Mandatorios**: por políticas inútiles (ej. javadocs forzados).
- **Comentarios de diario**: historia que debería ir en el sistema de control de versiones.
- **Ruido**: comentarios vacíos o decorativos.
- **Código comentado**: simplemente debería eliminarse.
- **Comentarios HTML**: agregan ruido innecesario.

---

## Capítulo 5: Formateo

### El Código es para Leerlo
- El formato debe maximizar la legibilidad y consistencia.

### Regla del Diario (Newspaper Metaphor)
- De arriba a abajo: primero lo importante, luego detalles.
- Como un artículo de diario: encabezado, resumen, detalles.

### Verticalidad
- **Espacio vertical entre conceptos distintos**.
- **Densidad vertical entre elementos relacionados**.
- Ejemplo:
```ts
// Separar funciones por una línea en blanco
function getName() {}
function getAge() {}
```

### Orden Vertical
- Las funciones de alto nivel deben ir arriba, las de bajo nivel debajo.
- Se debe poder leer de arriba hacia abajo de forma natural.

### Horizontalidad
- Indentación consistente y mínima.
- Las líneas deben ser cortas y claras.

---

## Capítulo 6: Objetos y Estructuras de Datos

### Abstracción de Datos
- Ocultar implementación, exponer solo lo necesario.
- Ejemplo (malo):
```ts
class Point {
  public x: number;
  public y: number;
}
```
- Ejemplo (bueno):
```ts
class Point {
  constructor(private x: number, private y: number) {}
  getX() { return this.x; }
  getY() { return this.y; }
  setCartesian(x: number, y: number) {
    this.x = x;
    this.y = y;
  }
}
```

### Anti-simetría Objeto/Dato
- **Objetos**: exponen comportamiento, ocultan datos.
- **Estructuras de datos**: exponen datos, no comportamiento.

### Procedural vs OO
- Código **procedural**: fácil de agregar funciones nuevas, difícil de agregar estructuras nuevas.
- Código **OO**: fácil de agregar estructuras nuevas, difícil de agregar funciones nuevas.

### DTO (Data Transfer Object)
- Clase con datos públicos y sin lógica.
```ts
interface AddressDTO {
  street: string;
  city: string;
  zip: string;
}
```

### Active Record
- DTO que contiene lógica de persistencia como `save()` o `find()`.
- No debe contener reglas de negocio.

### Ley de Demeter
- “Habla con tus amigos, no con extraños”.
- Ejemplo malo:
```ts
// Violación: encadenamiento de llamadas
const dir = ctxt.getOptions().getScratchDir().getAbsolutePath();
```
- Ejemplo mejor:
```ts
const dir = ctxt.createScratchFileStream(fileName);
```


