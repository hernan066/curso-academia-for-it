# Clean Code: Resumen de los Capítulos 7 al 13

📃 Esto es un resumen de los conceptos claves, del capitulo 7 al 13.

## Capítulo 7: Manejo de Errores

### Usar Excepciones en lugar de Códigos de Retorno
- Los códigos de error ensucian la lógica del programa.
- Las excepciones permiten separar la lógica principal del manejo de errores.
```ts
function readFile(path: string): string {
  if (!path) throw new Error("Path inválido");
  return fs.readFileSync(path, "utf-8");
}
```

### Escribe Primero el try-catch-finally
- Pensar primero en cómo manejar errores ayuda a escribir código más robusto.
```ts
try {
  const data = readFile("config.json");
  console.log(data);
} catch (e) {
  console.error("Error al leer archivo", e);
}
```

### Usa Excepciones Personalizadas
```ts
class StorageException extends Error {
  constructor(message: string) {
    super(message);
    this.name = "StorageException";
  }
}
```

### No Atrapes Excepciones que No Puedes Manejar
- Solo captura si puedes tomar una acción útil.

### Principio de Separación
- Manejo de errores debe estar separado de la lógica de negocio.

### No Retornar null
- Prefiere lanzar excepciones antes que retornar `null` o `undefined`.

---

## Capítulo 8: Límites (Boundaries)

### Trabajar con Código de Terceros
- Usa envoltorios (wrappers) para limitar el impacto de bibliotecas externas.

```ts
class SensorMap {
  private sensors: Map<string, Sensor> = new Map();

  getSensor(id: string): Sensor | undefined {
    return this.sensors.get(id);
  }
}
```

### Interfaces Propias
- Define interfaces propias sobre componentes externos para mantener el control.

```ts
interface Transmitter {
  transmit(frequency: number, data: string): void;
}
```

### Técnicas para Trabajar con Código Aún No Existente
- Define lo que **quisieras tener** (interface deseada), luego implementa un adaptador.

### Tests en los Límites
- Asegura que lo que esperas de una librería externa siga siendo válido tras actualizaciones.

---

## Capítulo 9: Pruebas Unitarias

### Tests Limpios
- **Lectura** es lo más importante: claridad, simplicidad, expresividad.

### Patrones: BUILD - OPERATE - CHECK
```ts
test("cuando la temperatura es muy baja, activa alarma", () => {
  // BUILD
  const controller = new EnvironmentController();

  // OPERATE
  controller.setTemperature(-20);

  // CHECK
  expect(controller.alarm.lowTemp).toBe(true);
});
```

### Un Solo assert por Test
- Mejora la claridad y el diagnóstico de fallos.

### Concepto Único por Test
- Evitar pruebas que validan múltiples aspectos.

### Lenguaje de Dominio de Pruebas
- Crear funciones que abstraen detalles y facilitan la lectura de tests.

### F.I.R.S.T.
- **Fast**: rápidos  
- **Independent**: independientes  
- **Repeatable**: repetibles  
- **Self-validating**: booleanos (pass/fail)  
- **Timely**: antes del código de producción

---

## Capítulo 10: Clases

### Clases Deben Ser Pequeñas
- Primera regla: clases pequeñas. Segunda regla: aún más pequeñas.
- Medida: **número de responsabilidades** (Single Responsibility Principle).

### Organización de una Clase
1. Constantes públicas
2. Variables privadas estáticas
3. Variables privadas de instancia
4. Métodos públicos
5. Métodos privados

### Encapsulamiento
- Priorizar `private` para métodos y propiedades.
- Solo flexibilizar (`protected`/`package`) si lo necesita un test.

### Alta Cohesión
- Las variables de instancia deben ser utilizadas por la mayoría de los métodos.
- Si no, es señal de que esa lógica debería estar en otra clase.

### Ejemplo TypeScript

```ts
class Version {
  constructor(private major: number, private minor: number, private build: number) {}

  getMajor() { return this.major; }
  getMinor() { return this.minor; }
  getBuild() { return this.build; }
}
```

---

## Capítulo 11: Sistemas

### Separar Construcción del Uso
- La construcción (instanciación de objetos, wiring) debe estar separada del uso.
```ts
// main.ts
const controller = new OrderController(new OrderService(new OrderRepository()));
```

### Patrones de Construcción
- **Factory Pattern**: delega el `new` a una fábrica.
- **Dependency Injection (DI)**: invierte el control sobre las dependencias.

### Cross-Cutting Concerns
- Usar inyección, AOP, y configuración externa (como Spring) para aspectos como logging, seguridad, etc.

### Principios para Escalar
- Mantener bajo acoplamiento
- Interfaces para abstraer
- Uso estratégico de frameworks sin acoplamiento rígido

---

## Capítulo 12: Emergencia (Diseño Emergente)

Basado en las 4 Reglas del Diseño Simple de Kent Beck:

1. **Corre todos los tests**
2. **No hay duplicación**
3. **Expresa la intención del programador**
4. **Minimiza clases y métodos**

### Refactorización Continua
- Quitar duplicación, hacer expresivo, y reducir tamaño constantemente.

### Ejemplo
```ts
// Si esto se repite...
function formatPrice(p: number) {
  return `$${p.toFixed(2)}`;
}

// Extraer
function formatCurrency(value: number, symbol = "$"): string {
  return `${symbol}${value.toFixed(2)}`;
}
```

---

## Capítulo 13: Concurrencia

### ¿Por Qué Usar Concurrencia?
- Decoupling: separar qué se hace de cuándo se hace.
- Mejorar throughput en tareas I/O o independientes.

### Dificultades
- Carreras, interbloqueos, orden no determinístico.
- Código concurrente puede fallar de forma intermitente y difícil de reproducir.

### Principios para Código Concurrente Limpio
- SRP: clases con una sola razón de cambiar
- Limitar alcance de los datos compartidos
- Usar copias de datos
- Hilos deben ser lo más independientes posible

### Ejemplo con `Worker` en Node
```ts
import { Worker } from 'worker_threads';

const worker = new Worker('./my-worker.js');
worker.postMessage({ data: 'process this' });
```

### Testing Concurrente
- Instrumentar para forzar fallos
- Ejecutar con múltiples hilos/procesos/plataformas






