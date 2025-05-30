# 📘 Guía de Estudio: TypeScript - Parte 4: Más tipos y guardas

## 📌 Tema: Tipos utilitarios, análisis de flujo y guardas de tipo

---

## 🧩 Conceptos clave

- Tipos utilitarios: `Record`, `Awaited`, `Partial`, `Readonly`, `Pick`, `Omit`
- Guardas de tipo: `typeof`, `instanceof`, `in`
- Predicados de tipo personalizados
- Uniones discriminadas y exhaustividad

---

## 🛠️ Tipos utilitarios

### `Record`

```ts
type Diccionario = Record<string, number>;

const puntajes: Diccionario = {
  juan: 10,
  ana: 15
};
```

- Llaves de un tipo dado, con valores homogéneos.

### `Awaited`

```ts
type Respuesta = Promise<string>;
type Valor = Awaited<Respuesta>; // string
```

- Extrae el tipo que resuelve una promesa.

### `Partial`

```ts
interface Usuario {
  nombre: string;
  edad: number;
}

type UsuarioParcial = Partial<Usuario>;
```

- Convierte todas las propiedades en opcionales.

### `Readonly`

```ts
type UsuarioSoloLectura = Readonly<Usuario>;
```

- Hace que las propiedades no puedan modificarse.

### `Pick` y `Omit`

```ts
type UsuarioNombre = Pick<Usuario, "nombre">;
type UsuarioSinEdad = Omit<Usuario, "edad">;
```

- `Pick`: selecciona campos.
- `Omit`: excluye campos.

---

## 🔍 Guardas de tipo (Type Guards)

### `typeof`

```ts
function manejar(x: string | number) {
  if (typeof x === "string") {
    return x.toUpperCase();
  }
}
```

### `instanceof`

```ts
if (error instanceof Error) {
  console.log(error.message);
}
```

### `in`

```ts
type Gato = { maullar: () => void };
type Perro = { ladrar: () => void };
type Animal = Gato | Perro;

function hablar(animal: Animal) {
  if ("maullar" in animal) {
    animal.maullar();
  }
}
```

- Estrecha el tipo según si una propiedad existe o no.

---

## 🧠 Inferencia por asignación

El TypeScript intenta inferir el tipo más específico posible luego de una asignación clara, útil para contextos intermedios:

```ts
let estado = "ok"; // inferred as "ok" (literal type)
```

---

## ✅ Predicados de tipo

```ts
interface Pez { nadar: () => void; }
interface Pájaro { volar: () => void; }

function esPez(animal: Pez | Pájaro): animal is Pez {
  return (animal as Pez).nadar !== undefined;
}

function mover(animal: Pez | Pájaro) {
  if (esPez(animal)) {
    animal.nadar(); // TS sabe que es Pez
  } else {
    animal.volar();
  }
}
```

- Permite crear funciones personalizadas para estrechar tipos.

---

## 🧩 Uniones discriminadas

```ts
type Circulo = { tipo: "circulo", radio: number };
type Cuadrado = { tipo: "cuadrado", lado: number };
type Figura = Circulo | Cuadrado;

function area(figura: Figura) {
  switch (figura.tipo) {
    case "circulo":
      return Math.PI * figura.radio ** 2;
    case "cuadrado":
      return figura.lado ** 2;
    default:
      const _exhaustiva: never = figura;
      return _exhaustiva;
  }
}
```

- `tipo` es la propiedad discriminante.
- Se usa `switch` para aplicar lógica según el tipo.
- El `default` con tipo `never` asegura exhaustividad.

---

## 💡 Notas y buenas prácticas

- Los tipos utilitarios simplifican la manipulación de estructuras complejas.
- Las guardas son fundamentales para manejar tipos unión con seguridad.
- Usar predicados cuando la lógica de estrechamiento es compleja o se repite.
- Las uniones discriminadas facilitan lógica clara y exhaustiva.

---

## ❓Preguntas de repaso

1. ¿Para qué se usa el tipo `Record`?
2. ¿Cuál es la diferencia entre `Partial` y `Readonly`?
3. ¿Qué hace el operador `in` en una guarda de tipo?
4. ¿Cómo se define un predicado de tipo personalizado?
5. ¿Qué ventaja tiene el patrón de uniones discriminadas?
6. ¿Qué rol cumple el tipo `never` en una función `switch` exhaustiva?

