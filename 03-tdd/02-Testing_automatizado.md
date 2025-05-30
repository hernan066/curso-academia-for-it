# Guía de Estudio: Testing Automatizado y Vite + Vitest

## 🧠 Conceptos Fundamentales

### ¿Qué es el Testing Automatizado?

- Proceso de **escribir código que verifica automáticamente** que nuestro software funciona correctamente.
- En un proyecto con tests automatizados existen **dos partes principales**:
  - **Código de producción**: el que usan los usuarios.
  - **Código de prueba**: el que valida que el código de producción funciona como se espera.

### Definiciones clave:

- **Test**: una unidad de verificación automática.
- **Suite de tests**: conjunto de pruebas automatizadas de un proyecto.

---

## 🗂️ Organización del Código de Pruebas

### Colocalización de tests

- Consiste en **ubicar los tests cerca del código que prueban**, idealmente en el mismo archivo.
- Ventajas:
  - Más fácil de mantener y entender.
  - Facilita detectar código no testeado.
  - Permite exclusión más simple del código de test en el build final.

### Alternativas

- En algunos lenguajes o configuraciones, los tests pueden ir en **archivos o repositorios separados**.
- En este curso se adopta **colocalización en archivos separados** con **nomenclatura clara** (`.test.ts`).

---

## ⚙️ Herramientas y Configuración

### Librería utilizada: **Vitest**

- Rápida, eficiente y fácil de usar con TypeScript.
- Se instala como **dependencia de desarrollo** (`devDependency`).

### Configuración básica:

1. Instalar `vitest` y `tsconfig` si es necesario.
2. Agregar script en `package.json`:
   ```json
   "scripts": {
     "test": "vitest"
   }
   ```
3. Nombrar los archivos de test como: `nombre.test.ts` o `nombre.spec.ts`.

### Ejecutar tests

- `npm test`: inicia los tests en modo watch.
- `vitest run`: ejecuta una sola vez.
- Salir con `Ctrl + C` o `Q`.

---

## 🧪 Escribiendo Tests en Vitest

```ts
import { describe, test, expect } from "vitest";
import { zoom } from "./zoom";

describe("zoom", () => {
  test("devuelve 10 cuando se pasa 5 y 2", () => {
    expect(zoom(5, 2)).toBe(10);
  });

  test("devuelve 20 cuando se pasa 4 y 5", () => {
    expect(zoom(4, 5)).toBe(20);
  });
});
```

### Componentes principales

- `describe`: agrupa tests relacionados.
- `test`: define un caso de prueba.
- `expect`: realiza una aserción sobre el resultado.

---

## 📦 Excluir Tests del Build Final

### Objetivo

- Asegurar que **el código de producción no incluya tests ni sus dependencias**.

### Pasos:

1. En `tsconfig.json` principal:

   ```json
   {
     "compilerOptions": {
       "noEmit": true
     }
   }
   ```

2. Crear un `tsconfig.build.json`:

   ```json
   {
     "extends": "./tsconfig.json",
     "compilerOptions": {
       "noEmit": false,
       "outDir": "./dist"
     },
     "exclude": ["**/*.test.ts", "**/*.spec.ts"]
   }
   ```

3. Script de build en `package.json`:
   ```json
   "scripts": {
     "build": "tsc -p tsconfig.build.json"
   }
   ```

---

## 🚀 Aplicación en un Proyecto

1. Implementar función/producto mínimo.
2. Escribir tests que cubran su funcionamiento.
3. Ejecutar los tests para validación automática.
4. Excluir tests del build final para producción.

---

## 📚 Recomendación

Explorar la [documentación oficial de Vitest](https://vitest.dev) para conocer:

- Mocking.
- Setup global.
- Testing asíncrono.
- Cobertura de código.

---
