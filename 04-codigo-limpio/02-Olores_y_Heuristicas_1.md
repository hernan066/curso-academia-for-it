# Guía de Estudio: Olores y Heuristicas Pt.1

## 📝 Comentarios: ¿Cuándo sí y cuándo no?

### ❌ Mal uso de comentarios

- No deben contener información que ya existe en el sistema de tareas (Jira, Trello, etc.).
- Evitá dejar **comentarios obsoletos** o incorrectos. Generan confusión.
- Evitá **repetir lo que ya dice el código**.
- Comentarios mal escritos o con errores de ortografía generan más ruido que ayuda.
- Comentarios que "comentan código" suelen esconder **código muerto**.

### ✅ Buenas prácticas

- Preferí **no comentar** antes que dejar malos comentarios.
- Si necesitás dejar un comentario, asegurate de que:
  - Sea claro y conciso.
  - Agregue valor que no sea evidente en el código.
  - Esté bien redactado y sin errores.
- Si se comenta una línea de código "para el futuro", mejor: **crear una tarea y eliminar el código**.

---

## 🔠 Nombres: Claridad, Consistencia y Contexto

### 🧠 Elegí nombres descriptivos y significativos

- En lugar de `x`, usá `numberOfItems`.
- En lugar de `processData`, usá `validateAndProcessOrder`.

### 📐 Nivel de abstracción

- Usá nombres que correspondan con el nivel del código. Por ejemplo: `calculateCartTotal`, no `sumPricesWithDiscountIfExists`.

### 📏 Convenciones de nomenclatura (TypeScript)

- `camelCase`: variables, funciones.
- `PascalCase`: clases, interfaces.
- `SCREAMING_SNAKE_CASE`: constantes globales.
- `kebab-case`: nombres de archivos.

### 🚫 Evitá ambigüedades

- Si es una sola cosa, nombrá en **singular** (`product`, no `products`).

---

## 🧱 Funciones: Simples, Focalizadas y Claras

### 🔢 Argumentos

- Evitá funciones con muchos parámetros.
- Si los parámetros están relacionados, agrupalos en un **objeto**.
- Si la función hace muchas cosas, **dividila**.

### 🏁 Flags booleanas

- Las flags (`true/false`) suelen indicar que la función **viola la SRP** (Responsabilidad Única).
- Dividí la función en varias más simples.

### 🧹 Eliminación de funciones muertas

- Si una función no se llama: **eliminála**.

### 🧭 Nivel de abstracción coherente

- No mezcles lógica de alto nivel con detalles de bajo nivel en la misma función.
- Cada función debe operar en un único nivel de detalle.

---

## 🔀 Condicionales: Claridad ante todo

### 🧠 Refactorizá condiciones complejas

- Extraé condiciones en funciones descriptivas.

```ts
// En vez de:
if (user.isActive && !user.hasPendingInvoice)

// Mejor:
if (canUserAccessDashboard(user))
```

### ➕ Preferí condicionales positivos

- `if (isAuthorized)` en lugar de `if (!isUnauthorized)`.

---
