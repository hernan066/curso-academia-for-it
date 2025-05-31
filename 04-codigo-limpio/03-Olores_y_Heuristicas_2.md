# Guía de Estudio: Olores y Heuristicas Pt.2

## 🔍 Programar es Explorar (pero con límites)

- A menudo exploramos soluciones escribiendo código y validándolas con **tests unitarios**.
- TDD permite que **la solución emerja** a partir de los tests, pero no debemos depender completamente de la improvisación.
- Es vital **comprender los algoritmos antes de modificarlos**. Ejemplo: no toques una función que optimiza con raíz cuadrada si no entendés qué es un número primo.
- Con IA, esto es aún más crítico: **vos guiás a la IA, no al revés**.

---

## 🌐 Organización del Código

- Evitá **mezclar múltiples lenguajes** en un solo archivo.
- **Nunca combines idiomas humanos**. Si programás en inglés, los nombres y comentarios deben estar en inglés.

---

## 🧩 Comportamiento claro y esperado

- Tu código debe **hacer lo que sugiere su nombre**.
- Si una función se llama `getAreaOfCircle`, no debería devolver `undefined` ni hacer otra cosa inesperada.
- Si está incompleta, lanzá un error explícito (`throw new Error("Not implemented")`).

---

## 🧪 Casos límite y validaciones

- Considerá siempre:
  - Arrays vacíos
  - Números negativos
  - Índices fuera de rango
  - División por 0

```ts
function divide(a: number, b: number) {
  if (b === 0) throw new Error("Divisor cannot be zero");
  return a / b;
}
```

- Cada lenguaje maneja estos casos de forma diferente (ej: JS devuelve `Infinity` al dividir por 0).

---

## 🧼 Limpieza y orden en todo

- **Formateá tu código** con herramientas como **Prettier**.
- Usá nombres claros, estructura lógica, commits descriptivos y ramas bien nombradas.
- El desorden es señal de falta de profesionalismo.

---

## 💭 Decisiones bien justificadas

- Evitá decisiones arbitrarias en el código.
- Si algo no es evidente, **documentá la decisión**.

---

## ⚙️ Build y scripts

- El proceso de build debe ser simple y automático.
- Uní pasos múltiples en scripts (ej: NPM) para evitar errores.

```json
"scripts": {
  "build": "tsc && mv dist/index.js ./out/"
}
```

---

## 🧰 Herramientas y convenciones

- Usá **linters** como ESLint y **formatters** como Prettier.
- Respetá las **convenciones de tu lenguaje/framework**.

---

## ⚙️ Configuración y variables de entorno

- Mantené la configuración en archivos centrales y bien documentados.
- No accedas a variables de entorno en módulos aislados, pasalas como parámetros desde un punto de entrada.

---

## 🛡️ TypeScript: Seguridad y claridad

- **Evitá `any`**, preferí `unknown` o tipos específicos.
- Definí los tipos **en los boundaries** de tu sistema (interfaces públicas, funciones exportadas).

```ts
export function login(user: UserCredentials): Promise<AuthResult> { ... }
```

- No ignores reglas sin justificarlo (`ts-ignore`, `eslint-disable`).
- TypeScript puede inferir muchos tipos, pero **sé explícito en las APIs**.

---

## 📈 Cobertura de código

- Usá herramientas de **coverage** para saber qué código está siendo testeado.
- En Vitest: `vitest run --coverage`
- Esto te ayuda a detectar:
  - Código sin pruebas.
  - Lógica no necesaria (que no fue exigida por los tests).

---

## ⚡ Velocidad de los tests

- Los tests deben ser **rápidos**.
- Tests lentos → Menos ejecuciones → Menos feedback.
- Si hay tests lentos inevitables, filtralos con variables de entorno y documentalo.

---
