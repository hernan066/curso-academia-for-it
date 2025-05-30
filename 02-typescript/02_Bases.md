# 📘 Guía de Estudio: TypeScript - Parte 2: Bases

## 📌 Tema: Configuración inicial y sistema de tipos

---

## 🧩 Conceptos clave

- Instalación de TypeScript y configuración de proyecto.
- Transpilación vs Compilación.
- Archivo `tsconfig.json` y configuración del compilador.
- Tipado estático, inferencia, uso de `any`.
- Tipos primitivos, arrays, tuplas, objetos (interfaces) y funciones.

---

## ⚙️ Configuración inicial del proyecto

1. Inicializar proyecto:
   ```bash
   npm init -y
   ```

2. Instalar TypeScript como dependencia de desarrollo:
   ```bash
   npm i -D typescript
   ```

3. Crear archivo de entrada:
   ```bash
   touch index.ts
   ```

4. Transpilar a JavaScript:
   ```bash
   npx tsc index.ts
   ```

✅ Esto genera un archivo `index.js` con código JavaScript compatible.

---

## 📁 tsconfig.json

Crear archivo de configuración:

```bash
npx tsc --init
```

Configuraciones clave:

```json
{
  "outDir": "./dist",
  "rootDir": "./src",
  "target": "esnext",
  "strict": true
}
```

- `outDir`: carpeta de salida (`dist`).
- `rootDir`: carpeta fuente (`src`).
- `target`: versión de JavaScript a generar.
- `strict`: activa todos los chequeos de tipo opcionales.

---

## 🔠 Tipado en TypeScript

### Definición explícita:
```ts
let nombre: string = "Juan";
let edad: number = 30;
let activo: boolean = true;
```

### Inferencia de tipos:
```ts
let saludo = "Hola"; // TypeScript infiere string
```

### Uso de `any` (evitar):
```ts
let dato: any = "puede ser cualquier cosa";
```

---

## 🧪 Tipos básicos

- `string`, `number`, `boolean`, `null`, `undefined`, `symbol`
- Clases: `Date`, `URL`, `File`, etc.

### Arrays y Tuplas

```ts
let numeros: number[] = [1, 2, 3];
let tupla: [string, number] = ["edad", 42];
```

---

## 🧩 Interfaces

```ts
interface Usuario {
  nombre: string;
  edad: number;
  activo: boolean;
}

const user: Usuario = {
  nombre: "Ana",
  edad: 28,
  activo: true
};
```

---

## 🧮 Funciones tipadas

```ts
function sumar(a: number, b: number): number {
  return a + b;
}
```

❗ Con `strict`, todos los parámetros y retornos deben tener tipos explícitos.

### Inferencia del tipo de retorno (no recomendado):
```ts
function saludar(nombre: string) {
  return "Hola " + nombre;
}
```

### Mejor práctica:
```ts
function saludar(nombre: string): string {
  return "Hola " + nombre;
}
```

💡 En VS Code se puede usar `Ctrl + .` para inferir automáticamente el tipo de retorno.

---

## 💡 Notas y buenas prácticas

- Siempre usar `"strict": true` para aprovechar al máximo TypeScript.
- Preferir inferencia solo para variables simples, pero tipar explícitamente argumentos y retornos de funciones.
- Usar interfaces para representar objetos complejos.
- Evitar `any` salvo en situaciones de transición.

---

## ❓Preguntas de repaso

1. ¿Cuál es el comando para instalar TypeScript como dependencia de desarrollo?
2. ¿Qué archivo permite configurar cómo se transpila TypeScript?
3. ¿Qué significa el modo `strict`?
4. ¿Qué diferencia hay entre inferencia y definición explícita de tipos?
5. ¿Qué estructura usamos para tipar objetos?
6. ¿Por qué es importante tipar el retorno de funciones públicas?

