# Guía de Estudio: Introducción al Testing y TDD

## 🧠 Conceptos Clave

### ¿Qué es el testing?

- Es el proceso de **ejecutar software intencionalmente para encontrar fallas**.
- Es una **parte esencial del desarrollo**: garantiza el correcto funcionamiento y cumplimiento de expectativas.

### ¿Por qué es crítico testear el software?

- No hacerlo es considerado **negligente** con:
  - Usuarios.
  - Empleadores.
  - Nosotros mismos.
- El testing permite descubrir errores antes de que impacten en producción.

### Problemas del testing manual

- **Subjetividad y distracción** pueden provocar que dejemos pasar fallas.
- A medida que el proyecto crece, **aumenta la complejidad** y el tiempo necesario para probar.
- Es **ineficiente** intentar probar todo manualmente tras cada cambio.

### Ejemplos típicos:

- Cambios pequeños que generan errores en otras partes.
- Nuevos integrantes del equipo sin conocimiento del sistema completo.
- Omisión de casos de prueba por error humano.

## 🛠️ Solución: Automatización del testing

### Por qué automatizar

- Garantiza que **cada parte del software sea probada en cada cambio**.
- Elimina el error humano, permite pruebas **rápidas, completas y confiables**.
- Escala con el proyecto.

### ¿Cómo automatizar de forma efectiva?

- Escribiendo software que **prueba el software**.
- Esto implica implementar pruebas automatizadas como parte integral del desarrollo.

## ✅ Introducción a TDD (Test Driven Development)

### ¿Qué es TDD?

- Técnica de desarrollo en la que primero se escriben las pruebas y luego el código que las hace pasar.
- Ciclo:
  1. **Red** → Escribir una prueba que falla.
  2. **Green** → Escribir el código mínimo necesario para que pase.
  3. **Refactor** → Mejorar el código manteniendo la prueba verde.

### Beneficios de TDD:

- Fomenta diseño limpio y código mantenible.
- Documenta las expectativas del sistema.
- Mejora la **confianza para hacer cambios futuros**.

## 🚀 Aplicación en un proyecto

### Etapas:

1. **Antes de programar una nueva funcionalidad**, escribir una prueba que verifique su comportamiento.
2. Codificar la solución que haga pasar esa prueba.
3. Refactorizar sin temor a romper algo, ya que las pruebas actúan como red de seguridad.
4. Integrar el sistema de testing en el CI/CD para validar automáticamente cada cambio.

### Herramientas comunes:

- **Jest**, **Mocha**, **Vitest** (JavaScript/TypeScript).
- **JUnit** (Java).
- **PyTest** (Python).
- **Postman/Newman** o **Supertest** para APIs REST.
