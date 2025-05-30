# 📘 Guía de Estudio: TypeScript - Introducción

## 📌 Tema: ¿Por qué TypeScript? Origen y propósito

---

## 🧩 Conceptos clave

- JavaScript es el lenguaje dominante en la web.
- TypeScript surge como una evolución necesaria para proyectos más grandes y complejos.
- Introducido por Microsoft en 2012.
- Añade **tipado estático** sobre JavaScript.
- Mejora la **experiencia de desarrollo**: autocompletado, refactorización, errores en tiempo de desarrollo.
- Se **transpila** a JavaScript para mantener compatibilidad con todos los navegadores.

---

## 📖 Resumen del video

JavaScript es el lenguaje más usado en el desarrollo de software gracias a su relación con la web, que es la plataforma universal para distribuir aplicaciones. Esta ubicuidad lo convirtió en la opción por defecto para desarrollar interfaces y servicios.

Pero esta versatilidad tiene un costo: a medida que las aplicaciones web crecieron en tamaño y complejidad, el carácter dinámico de JavaScript se volvió problemático. La falta de tipos explícitos, sumado a errores que solo se detectaban en tiempo de ejecución, dificultaba mantener código escalable, predecible y seguro.

En 2012, **Microsoft lanza TypeScript**, un superset de JavaScript que agrega **tipado estático opcional**, buscando solucionar estos problemas y facilitar el desarrollo de grandes aplicaciones. TypeScript se integra profundamente con herramientas como **Visual Studio Code**, permitiendo al programador escribir código con soporte de autocompletado, navegación entre símbolos, refactors y verificación de tipos sin ejecutar el programa.

Además, permite usar nuevas funcionalidades de JavaScript (como `async/await`, clases, módulos) y transpilar a versiones anteriores de ECMAScript compatibles con navegadores más antiguos.

---

## 🧪 Ejemplo

**JavaScript tradicional:**

```js
function saludar(nombre) {
  return "Hola " + nombre.toUpperCase();
}
```

**Problema:** Si alguien llama `saludar(null)`, el error `TypeError: Cannot read property 'toUpperCase' of null` ocurrirá en tiempo de ejecución.

**TypeScript:**

```ts
function saludar(nombre: string): string {
  return "Hola " + nombre.toUpperCase();
}
```

✅ **Beneficio:** TypeScript detectará un error en tiempo de desarrollo si `nombre` no es de tipo `string`.

---

## 💡 Notas y buenas prácticas

- TypeScript **no reemplaza** a JavaScript: todo código JavaScript válido es también válido en TypeScript.
- El **tipado estático** es opcional pero recomendado en proyectos grandes.
- Al compilar, TypeScript genera archivos `.js` que pueden ser usados como cualquier script en el navegador.
- La curva de aprendizaje es muy baja si ya sabés JavaScript.

---

## ❓Preguntas de repaso

1. ¿Por qué JavaScript se volvió el lenguaje más usado en la industria?
2. ¿Qué problemas surgen al usar JavaScript en proyectos grandes?
3. ¿Qué es TypeScript y quién lo desarrolló?
4. ¿Cuáles son las ventajas del tipado estático?
5. ¿Qué herramientas se potencian al usar TypeScript?
