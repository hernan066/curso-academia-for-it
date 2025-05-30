# Guía de Estudio: Aplicación Práctica de TDD con FizzBuzz

## 🔁 Revisión de conceptos clave

- **Escribimos software que prueba software.** Pero… ¿quién prueba ese software de prueba?
- La única forma de confiar en que el software funciona correctamente es **automatizando las pruebas** y **probando las pruebas**.
- Para eso necesitamos una técnica como **TDD (Test Driven Development)**.

---

## 📖 ¿Qué es TDD y cómo se aplica?

- TDD es una técnica creada por **Ken Beck** en los años 90.
- En su libro _TDD by Example_, se presenta un ciclo de desarrollo simple y repetitivo para escribir tests confiables.
- **Este módulo no reemplaza el libro**, pero explora sus fundamentos y cómo aplicarlos con TypeScript.

---

## 🧪 Práctica con Katas: FizzBuzz

### ¿Por qué Katas?

- Son ejercicios de programación repetibles.
- Mejoran habilidades técnicas.
- Fomentan el aprendizaje práctico de nuevas técnicas.

---

## 🎯 Objetivo

Crear una función que dado un número devuelva:

- `"Fizz"` si es divisible por 3.
- `"Buzz"` si es divisible por 5.
- `"FizzBuzz"` si es divisible por ambos.
- El número si no cumple ninguna condición.

---

## 🔄 Ciclo TDD paso a paso

1. **RED**: Escribir un test que falle por la razón esperada.
2. **GREEN**: Escribir lo mínimo necesario para hacerlo pasar.
3. **REFACTOR**: Mejorar el código eliminando duplicaciones.

### Aplicación concreta:

- Escribir tests en Vitest utilizando `describe`, `test`, `expect`.
- Ver el test fallar (confirma que prueba algo real).
- Hacer lo mínimo para que pase.
- Repetir con el siguiente caso.

```ts
test("devuelve Fizz si es divisible por 3", () => {
  expect(fizzBuzz(3)).toBe("Fizz");
});
```

---

## 🛠️ Consejos prácticos

- Dividir tests complejos en partes simples.
- Primero pensar **cómo se va a usar la función** (firma, parámetros, retorno).
- La función inicialmente puede retornar valores “duros” (ej. `'Fizz'`).
- Ir incorporando lógica condicional a medida que los tests lo exijan.
- Usar `mod` para detectar divisibilidad.
- Refactorizar solo cuando todos los tests pasen.

---

## ✅ Buenas prácticas

- Validar que los errores sucedan por las razones esperadas.
- Hacer commit luego de cada test que pase.
- Revisar y eliminar duplicación en los chequeos.
- Repetir las catas e incorporar variaciones o restricciones para seguir aprendiendo.

---

## 🧠 Reflexión final

- Cada vez que realizás una kata podés mejorar tu enfoque.
- Evaluá qué fue fácil, difícil, qué mejorarías.
- Aplicá lo aprendido en nuevos desafíos.

---
