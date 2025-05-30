# 📘 Guía de Estudio: TypeScript - Parte 3: Tipos Básicos

## 📌 Tema: Tipos avanzados, aliases, uniones, literales y generics

---

## 🧩 Conceptos clave

- `any`, `unknown`, `void`, `never`
- Aliases de tipos
- Tipos unión y literales
- Propiedades opcionales vs undefined
- Generics (tipos genéricos) y restricciones

---

## 🔍 `any` vs `unknown`

```ts
let dato: any = "hola";    // Se puede hacer cualquier cosa => pierde seguridad
let valor: unknown = "test"; // Se debe verificar el tipo antes de usar

if (typeof valor === "string") {
  console.log(valor.toUpperCase());
}
```

- `any`: desactiva completamente el chequeo de tipos.
- `unknown`: requiere verificación de tipo (`type narrowing`) antes de su uso.

---

## 🔁 `void` y `never`

```ts
function log(mensaje: string): void {
  console.log(mensaje);
}

function error(msg: string): never {
  throw new Error(msg);
}
```

- `void`: función sin retorno.
- `never`: función que nunca retorna o lanza siempre error.

---

## 🏷️ Alias de tipos

```ts
type UsuarioId = string;
type Coordenadas = [number, number];
```

- Permite nombrar tipos complejos o reutilizables.

---

## 🔗 Tipos unión

```ts
type TextoONumero = string | number;

function imprimir(valor: TextoONumero) {
  if (typeof valor === "string") {
    console.log(valor.toUpperCase());
  }
}
```

- Permiten múltiples tipos posibles.
- Requiere type narrowing para operaciones específicas.

---

## 📦 Propiedades opcionales

```ts
interface Usuario {
  nombre: string;
  edad?: number; // opcional
}
```

```ts
interface UsuarioAlt {
  nombre: string;
  edad: number | undefined; // no opcional
}
```

- `edad?`: puede omitirse y puede ser `undefined`.
- `edad: number | undefined`: debe estar presente pero puede ser `undefined`.

---

## 🔒 Tipos literales

```ts
type Estado = "activo" | "inactivo" | "pendiente";

function cambiarEstado(estado: Estado) {
  // Solo acepta uno de los valores definidos
}
```

- Restringen el valor a un conjunto específico (como enums pero más simples).
- Muy útiles para representar estados.

---

## 🧬 Generics (tipos genéricos)

```ts
function envolverEnArray<TValor>(valor: TValor): TValor[] {
  return [valor];
}

const resultado = envolverEnArray(42); // inferred: number[]
```

- Generalizan la lógica sobre tipos sin perder seguridad.
- Se usa `<T>` o nombres descriptivos como `<TEntidad>`, `<TValor>`.

### Generics en interfaces:

```ts
interface Servicio<TEntidad> {
  guardar(entidad: TEntidad): void;
  obtener(id: string): TEntidad;
  obtenerTodos(): TEntidad[];
}
```

### Con restricciones:

```ts
interface ConId {
  id: string;
}

interface ServicioConRestriccion<TEntidad extends ConId> {
  guardar(entidad: TEntidad): void;
  obtener(id: string): TEntidad;
}
```

---

## 💡 Notas y buenas prácticas

- Preferir `unknown` sobre `any` para mantener el type safety.
- Usar `void` en funciones que no retornan nada.
- `never` ayuda al compilador a detectar ramas imposibles.
- Tipos unión + literales = estados seguros.
- Siempre nombrar genéricos con claridad: `TValor`, `TEntidad`, etc.
- Agregar restricciones a los generics cuando sea necesario.

---

## ❓Preguntas de repaso

1. ¿Cuál es la diferencia entre `any` y `unknown`?
2. ¿Para qué se usa `never` en una función?
3. ¿Cómo se declara un tipo unión? ¿Y un literal?
4. ¿Qué diferencia hay entre una propiedad opcional y una unión con `undefined`?
5. ¿Qué son los generics y cómo se usan?
6. ¿Cómo se limitan los valores posibles de un tipo?

