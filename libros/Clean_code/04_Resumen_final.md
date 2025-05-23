# Clean Code: Resumen del Capítulo 14 y Apéndices (Smells and Heuristics)

📃 Esto es un resumen de los conceptos claves, del capítulo 14 y apéndices.

## Capítulo 14: Refinamiento Sucesivo

### Args - Un Ejemplo Práctico
- Se implementa un parser de argumentos de línea de comandos.
- El diseño evoluciona a través de refactorizaciones incrementales.

### Principios Ilustrados
- **Diseño Incremental**: comenzar con una solución simple e ir agregando complejidad controladamente.
- **Eliminar Duplicación**: cada refactorización reduce redundancias.
- **Expresar Intención**: nombres, estructuras y tests ayudan a que el código sea legible.
- **Mínimo Necesario**: se eliminan funciones o estructuras innecesarias.

---

## Apéndice A y B: Smells and Heuristics

### Heurísticas Generales (G)
- **G5: Duplication**  
  Extraer lógica repetida mejora el diseño (DRY).
- **G10: Vertical Separation**  
  Conceptos distintos deben estar separados verticalmente en el código.
- **G20: Function Names Should Say What They Do**  
  Nombres claros y representativos de la acción.
- **G23: Prefer Polymorphism to If/Else or Switch/Case**  
  Usar herencia o interfaces en lugar de estructuras de control.

```ts
// Preferir esto:
abstract class Employee {
  abstract calculatePay(): number;
}

class HourlyEmployee extends Employee {
  calculatePay() { return 100; }
}
```

- **G25: Replace Magic Numbers with Named Constants**  
  ```ts
  const MAX_RETRIES = 3;
  ```

- **G30: Functions Should Do One Thing**  
  Funciones cortas y enfocadas en una sola responsabilidad.

### Heurísticas de Nombres (N)
- **N1: Choose Descriptive Names**
- **N6: Avoid Encodings**  
  No usar prefijos como `m_`, `sz`, etc.

### Heurísticas de Pruebas (T)
- **T1: Insufficient Tests**
- **T3: Don’t Skip Trivial Tests**
- **T9: Tests Should Be Fast**

---

## Olores Comunes

| Olor | Descripción | Mejora sugerida |
|------|-------------|-----------------|
| Código duplicado | Fragmentos idénticos | Extraer funciones |
| Clases largas | Demasiadas responsabilidades | Aplicar SRP |
| Métodos largos | Mucho código en una sola función | Dividir |
| Abstracciones inapropiadas | Herencia sin sentido | Usar composición |
| Encapsulamiento débil | Exposición innecesaria de datos | Usar `private` o `getter/setter` |
| Dependencias ocultas | Uso implícito de globals o mocks | Inyección de dependencias |
| Malos nombres | Variables/métodos poco claros | Renombrar por intención |



