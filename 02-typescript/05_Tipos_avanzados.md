# 📘 Guía de Estudio: TypeScript - Parte 5: Tipos Avanzados

## 📌 Tema: Recursividad, tipos condicionales, acceso indexado, enums simulados y más

---

## 🧩 Conceptos clave

- Tipos recursivos
- `keyof`, `as const`, `typeof`, acceso indexado
- Enums simulados
- Tipos de intersección
- Tipos condicionales e inferencia con `infer`

---

## 🌳 Tipos recursivos

```ts
type NodoArbol = {
  valor: number;
  izquierdo?: NodoArbol;
  derecho?: NodoArbol;
};
```

- Permite que un tipo se refiera a sí mismo.
- Ideal para estructuras como árboles, listas enlazadas, etc.

---

## 🧾 `keyof`, `typeof`, `as const`, acceso indexado

### Ejemplo:

```ts
const ROLES = {
  ADMIN: "admin",
  USER: "user",
  GUEST: "guest"
} as const;

type Rol = typeof ROLES[keyof typeof ROLES];
```

- `typeof ROLES`: toma el tipo del objeto.
- `keyof typeof ROLES`: obtiene las claves (`"ADMIN" | "USER" | "GUEST"`).
- `typeof ROLES[keyof typeof ROLES]`: obtiene los valores (`"admin" | "user" | "guest"`).
- `as const`: infiere los valores como literales en lugar de `string`.

---

## 🚫 Enums nativos de TypeScript

Aunque existen enums en TypeScript:

```ts
enum RolEnum {
  Admin = "admin",
  User = "user"
}
```

❌ No se recomienda usarlos porque:
- Generan código JavaScript adicional.
- Su comportamiento puede variar según la versión del compilador.
✅ Preferible usar objetos `as const` y tipos derivados.

---

## 🤝 Tipos de intersección

```ts
interface Persona {
  nombre: string;
  edad: number;
}

interface Credenciales {
  usuario: string;
  password: string;
}

type Usuario = Persona & Credenciales;
```

- Une múltiples tipos en uno solo.
- Todos los campos deben estar presentes.

---

## 🧮 Tipos condicionales

```ts
type Aplanar<T> = T extends Array<infer U> ? U : T;

type Resultado1 = Aplanar<string[]>; // string
type Resultado2 = Aplanar<number>;   // number
```

- Tipo ternario: `T extends U ? X : Y`
- `infer` permite extraer dinámicamente un tipo dentro de una condición.

---

## 💡 Notas y buenas prácticas

- Tipos recursivos: útiles pero deben manejarse con cuidado para evitar bucles infinitos.
- Simular enums con `as const` y `typeof` da mayor control.
- Intersecciones útiles para componer múltiples interfaces.
- `infer` es poderoso para manipular tipos genéricos complejos.
- Combinar utilitarios con condicionales permite crear tipos expresivos y seguros.

---

## ❓Preguntas de repaso

1. ¿Qué es un tipo recursivo y para qué se usa?
2. ¿Qué hace el operador `keyof`?
3. ¿Qué ventaja tiene usar `as const` al definir objetos?
4. ¿Qué es un tipo de intersección? ¿Cuándo lo usarías?
5. ¿Cómo funcionan los tipos condicionales?
6. ¿Qué es el operador `infer` y en qué contexto puede usarse?
7. ¿Por qué se desaconseja el uso de enums nativos en TypeScript?

