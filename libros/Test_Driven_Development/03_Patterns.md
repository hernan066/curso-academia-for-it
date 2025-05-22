# Test-Driven Development: By Example - Parte III: Patrones de TDD

En esta tercera parte del libro, Kent Beck deja de lado el desarrollo de nuevos sistemas para centrarse en **los patrones mentales y técnicos** que emergen al aplicar TDD de forma constante. La idea es **reflexionar, nombrar y formalizar** los comportamientos repetitivos que permiten tomar decisiones efectivas durante el ciclo Red → Green → Refactor.

Esta sección está dividida en patrones prácticos y análisis de los dos ejemplos anteriores (`Money` y `xUnit`) bajo esta nueva mirada.

---

## Capítulo 25: Test-Driven Development Patterns

### 🎯 Objetivo
Introducir una **tipología de patrones** observados durante el desarrollo con TDD. Estos patrones están agrupados según el estado del ciclo: barra roja, barra verde o refactorización.

### 🧩 Clasificación general

- **Red Bar Patterns**: qué hacer cuando una prueba falla.
- **Green Bar Patterns**: cómo avanzar una vez que una prueba pasa.
- **Refactor Patterns**: cómo mejorar el diseño sin romper funcionalidad.
- **Design Patterns**: cómo modelar soluciones a nivel de arquitectura.

Este capítulo actúa como índice conceptual para los siguientes, que desglosan cada grupo de patrones.

---

## Capítulo 26: Red Bar Patterns

### 🎯 Objetivo
Presentar patrones para enfrentar la **barra roja** en TDD, es decir, cuando un test **no pasa**. El libro no contiene ejemplos en este capitulo, o muy pocos, pero se agregaron para mayor comprensión de dichos patrones.

### 🧩 Patrones destacados

### 🔹 Starter Test

> Escribí un test extremadamente simple para iniciar el ciclo.

```ts
test("starter test: crear objeto Money", () => {
  const money = new Money(1, "USD");
  expect(money).toBeDefined(); // aún no se testea comportamiento
});
```

---

### 🔹 One Step Test

> Escribí un test que solo falle por una razón específica, no por múltiples cosas a la vez.

```ts
test("one step: multiplicar cantidad", () => {
  const five = new Money(5, "USD");
  const result = five.times(2);
  expect(result.amount).toBe(10); // solo testea la cantidad, no la moneda
});
```

---

### 🔹 Another Test

> Si no sabés cómo avanzar, escribí otro test. Puede darte una pista sobre el diseño.

```ts
test("another test: multiplicar por 3", () => {
  const five = new Money(5, "USD");
  const result = five.times(3);
  expect(result.amount).toBe(15); // ayuda a ver patrón de multiplicación
});
```

---

### 🔹 Regression Test

> Documenta un error que ocurrió antes para prevenir que se repita.

```ts
test("regression: suma de monedas fallaba con cantidades negativas", () => {
  const negative = new Money(-5, "USD");
  const result = negative.plus(new Money(10, "USD"));
  expect(result.amount).toBe(5); // comportamiento corregido
});
```

---

### 🔹 Break for Understanding

> Escribí un test para entender el comportamiento actual antes de modificarlo.

```ts
test("break for understanding: comportamiento actual de equals", () => {
  const a = new Money(5, "USD");
  const b = new Money(5, "USD");
  expect(a.equals(b)).toBe(true); // confirma la lógica antes de refactorizar
});
```

---

### 🔹 Explanation Test

> Usá un test como medio para expresar una idea o aclarar comportamiento esperado.

```ts
test("explanation: suma de dinero devuelve suma total con misma moneda", () => {
  const sum = new Money(5, "USD").plus(new Money(10, "USD"));
  expect(sum.amount).toBe(15);
  expect(sum.currency).toBe("USD");
});
```

---

### 🔹 Learning Test

> Escribí un test para entender cómo funciona una API o componente externo.

```ts
test("learning test: cómo funciona Date en JavaScript", () => {
  const date = new Date("2025-01-01T00:00:00Z");
  expect(date.getUTCFullYear()).toBe(2025); // aprendizaje sobre la API
});
```

---

Estos patrones ayudan a iniciar el proceso de TDD con confianza y claridad, especialmente cuando aún no se conoce la solución completa. Cada test tiene un propósito: explorar, proteger, comunicar o simplemente empezar.

---

##  Capítulo 27: Testing Patterns

Este capítulo describe patrones de pruebas que no solo ayudan a verificar el comportamiento del código, sino que también mejoran la comprensión, comunicación y mantenimiento del sistema. Kent Beck no se enfoca en tests específicos para una unidad, sino en patrones generales que **dan forma a cómo se escriben y se piensan los tests**.

---

### 🔸 Child Test

> “Hacelo funcionar primero, después probalo.”

Este patrón sugiere que cuando estás construyendo infraestructura (como un framework de testing), puede ser útil escribir primero código de producción simple (sin pruebas), luego probarlo desde una clase de test más amplia.

#### 📌 Importancia
Rompe con la idea estricta de escribir primero el test en casos donde construir el test requiere que algo ya funcione mínimamente.

---

### 🔸 Mock Object

> Usar objetos falsos para verificar que se hagan ciertas llamadas.

Los mocks permiten validar interacciones en lugar de resultados. Beck destaca que **la lógica no debe estar dentro del mock**; debe usarse para confirmar **si una llamada sucedió o no**.

#### 🧪 Ejemplo en TypeScript

```ts
class MockLogger {
  messages: string[] = [];
  log(msg: string) {
    this.messages.push(msg);
  }
}

test("mock object: se llama al logger", () => {
  const mock = new MockLogger();
  const service = new UserService(mock);
  service.createUser("test");
  expect(mock.messages).toContain("Usuario creado: test");
});
```

---

### 🔸 Self Shunt

> Cuando el test actúa como su propio doble (mock, stub, etc.).

#### 🧪 Ejemplo

```ts
class FakeClockTest {
  ticks: number[] = [];

  tick(time: number) {
    this.ticks.push(time);
  }

  testSomethingUsesTick() {
    const thing = new ThingThatCallsTick(this);
    thing.doSomething();
    expect(this.ticks).toEqual([42]);
  }
}
```

Esto permite verificar indirectamente el comportamiento, sin tener que construir un mock externo.

---

### 🔸 Log String

> Usar un string para registrar acciones en orden y luego verificarlo.

Este patrón ayuda a entender qué ocurrió primero y qué después.

#### 🧪 Ejemplo

```ts
class Logger {
  log = "";
  append(msg: string) { this.log += msg + " "; }
}

test("log string registra secuencia de eventos", () => {
  const logger = new Logger();
  logger.append("setUp");
  logger.append("testMethod");
  logger.append("tearDown");
  expect(logger.log.trim()).toBe("setUp testMethod tearDown");
});
```

---

### 🔸 Expected Exception

> Asegurarse de que una excepción ocurra cuando debe.

#### 🧪 Ejemplo en TypeScript

```ts
test("expected exception: divide por cero lanza error", () => {
  expect(() => divide(10, 0)).toThrow("No se puede dividir por cero");
});
```

Este patrón valida que el código **falle correctamente**, y puede reemplazar estructuras de control con tests más expresivos.

---

### 🔸 Crash Test Dummy

> ¿Cómo probás código de error que normalmente no se ejecuta?
Usá un objeto que, en vez de ejecutar su comportamiento real, lanza una excepción.

Este patrón es útil cuando se quiere simular errores como un disco lleno o una conexión fallida sin depender de condiciones reales del entorno.

#### 🧪 Ejemplo en TypeScript

```ts
class File {
  createNewFile(): void {
    // lógica real
  }
}

class FullFile extends File {
  override createNewFile(): void {
    throw new Error("Simulated disk full");
  }
}

test("crash test dummy: simula error al guardar archivo", () => {
  const file = new FullFile();
  expect(() => saveAs(file)).toThrow("Simulated disk full");
});

```

Similar a un Mock Object, pero en lugar de imitar todo el objeto, se sobrescribe solo el comportamiento relevante.

### 🔸 Broken Test

>¿Cómo retomás una sesión de desarrollo cuando estás solo?
Dejá un test roto antes de cerrar.

La idea es terminar tu sesión de trabajo dejando un test intencionalmente fallando. Cuando vuelvas, tendrás un punto claro desde donde retomar, sin tener que recordar en qué estabas.

#### 💡 Beneficio

- Actúa como un “marcador mental” de tu progreso.

- El test fallido te guía de inmediato hacia tu próxima tarea.

- Ayuda a mantener el ritmo en sesiones cortadas o con interrupciones.

> "Un test roto no hace el programa menos terminado; solo hace explícito su estado"

### 🔸 Clean Check-in

>¿Cómo terminás una sesión de trabajo en equipo? 
Asegurate de dejar todos los tests en verde antes de hacer commit.

Mientras que “Broken Test” es útil para trabajo individual, Clean Check-in es esencial en un equipo. Dejar un test fallando antes de hacer **git push** puede causar frustración y pérdida de confianza entre compañeros.

#### 💡 Buenas prácticas

- Ejecutá toda la suite antes de hacer commit (incluso si toma tiempo).

- No comentees tests para que pasen.

- Si algo falla al integrar, podés intentar arreglarlo, pero si lleva más de unos minutos: volvé atrás y repensá.

>"Check in más seguido” es una consecuencia positiva de esta práctica

---

## Capítulo 28: Green Bar Patterns

Este capítulo analiza los patrones que surgen **una vez que los tests están en verde**. El objetivo no es agregar nuevas pruebas, sino **cómo avanzar con el diseño del sistema**, manteniendo la simplicidad, evitando sobreingeniería y progresando paso a paso.

---

### 🔹 Fake It ('Til You Make It)

> Implementá una solución artificial que haga pasar el test.

Usado en etapas iniciales. Se devuelve una constante o comportamiento forzado.

#### 🧪 Ejemplo

```ts
times(multiplier: number): Money {
  return new Money(10, "USD"); // Hardcodeado para que el test pase
}
```

El patrón se rompe al agregar más tests que obligan a generalizar.

---

### 🔹 Triangulate

> Agregá un segundo test para romper una implementación “fake” y guiar una solución más general.

#### 🧪 Ejemplo

```ts
test("multiplicar por 3", () => {
  const five = new Money(5, "USD");
  const result = five.times(3);
  expect(result.amount).toBe(15);
});
```

Ahora ya no sirve devolver siempre `10`, obliga a calcular el resultado.

---

### 🔹 Obvious Implementation

> Si la implementación es evidente, hacela directamente.

Este patrón se aplica cuando el código necesario es tan claro que no vale la pena pasar por “Fake It” o “Triangulate”.

#### 🧪 Ejemplo

```ts
times(multiplier: number): Money {
  return new Money(this.amount * multiplier, this.currency);
}
```

No hay ambigüedad ni riesgo. El test lo valida claramente.

---

### 🔹 One to Many

> Cuando ves múltiples instancias de un patrón o comportamiento, abstraelo.

Este patrón guía hacia una generalización sin hacerlo prematuramente.

### 🧪 Ejemplo

```ts
// Paso previo
const sum = new Sum(new Money(5, "USD"), new Money(10, "USD"));

// Después de varias repeticiones
const sum = Money.dollar(5).plus(Money.dollar(10));
```

---

### 🔹 Triangulate Field

> Identificá qué campos deben convertirse en propiedades de objeto.

Cuando dos tests usan valores distintos, probablemente ese dato deba guardarse.

#### 🧪 Ejemplo

```ts
test("dólar y franco deben conservar moneda", () => {
  expect(new Money(5, "USD").currency).toBe("USD");
  expect(new Money(5, "CHF").currency).toBe("CHF");
});
```

Esto lleva a guardar `currency` como campo del objeto.

---

### 🔹 Useful Duplication

> Permití duplicación temporal si te permite avanzar. Refactorizá más tarde.

La duplicación puede ayudarte a observar patrones. Una vez que está clara, la refactorización se vuelve natural y segura.

#### 🧪 Ejemplo

```ts
// Primero
if (currency === "USD") { ... }
if (currency === "CHF") { ... }

// Luego
// currency se vuelve un campo, y se abstrae la lógica
```

---

## Capítulo 29: xUnit Patterns

Este capítulo describe los patrones que constituyen la arquitectura de un framework de testing del estilo xUnit. 

---

### 🔹 Assertion

> Validar que algo esperado ocurrió.

Las aserciones son la base de cualquier test. Se pueden implementar de forma simple o usar frameworks externos.

#### 🧪 Ejemplo

```ts
expect(2 + 2).toBe(4);
```

Un buen framework permite múltiples tipos de aserciones (`toBe`, `toEqual`, `toThrow`, etc.).

---

### 🔹 Fixture

> El conjunto de objetos que configuran el estado inicial del sistema bajo prueba.

Se prepara en `setUp()` y se limpia en `tearDown()`.

#### 🧪 Ejemplo

```ts
let calculator: Calculator;

beforeEach(() => {
  calculator = new Calculator();
});
```

Esto asegura que cada test empiece en un estado conocido.

---

### 🔹 External Fixture

> Cuando el estado de prueba necesita infraestructura externa, como una base de datos.

#### 🧠 Consideración
- Usá dobles de prueba si es posible (mocks, fakes).
- Separá lo más posible la lógica de negocio del acceso externo.

```ts
beforeEach(() => {
  mockDB = new InMemoryDatabase();
  service = new UserService(mockDB);
});
```

---

### 🔹 Test Method

> Cada test es un método separado que verifica una única cosa.

#### 🧪 Ejemplo

```ts
test("should multiply correctly", () => {
  const money = new Money(5, "USD");
  expect(money.times(2).amount).toBe(10);
});
```

Esto permite que los tests se ejecuten y reporten individualmente.

---

### 🔹 Test Case Class

> Agrupación de métodos de prueba relacionados.

#### 🧪 Ejemplo

```ts
class MoneyTests {
  testMultiplication() { ... }
  testEquality() { ... }
}
```

Agrupar ayuda a estructurar por contexto o unidad de negocio.

---

### 🔹 Test Suite

> Ejecutar múltiples casos de prueba juntos.

#### 🧪 Ejemplo

```ts
class TestSuite {
  private tests: TestCase[] = [];

  add(test: TestCase) {
    this.tests.push(test);
  }

  run(result: TestResult) {
    this.tests.forEach(test => test.run(result));
  }
}
```

Agrupa tests manualmente o dinámicamente.

---

### 🔹 Test Result

> Lleva el conteo de pruebas ejecutadas y fallidas.

#### 🧪 Ejemplo

```ts
class TestResult {
  private runs = 0;
  private fails = 0;

  testStarted() { this.runs++; }
  testFailed() { this.fails++; }

  summary(): string {
    return `${this.runs} run, ${this.fails} failed`;
  }
}
```

Permite reportes automatizados y feedback inmediato.

---

### 🔹 Pluggable Selector

> Ejecutar un método por su nombre (como string).

#### 🧪 Ejemplo

```ts
class TestCase {
  constructor(private name: string) {}

  run(result: TestResult) {
    const method = (this as any)[this.name];
    if (typeof method === "function") {
      method.call(this);
    }
    result.testStarted();
  }
}
```

Permite flexibilidad en qué prueba ejecutar dinámicamente.

---

### 🔹 Pluggable Object

> Pasar un objeto con lógica de test a un método o clase que lo ejecuta.

#### 🧠 Uso típico

```ts
suite.add(new WasRun("testMethod"));
```

Hace que el framework sea extensible y desacoplado.

---

### 🔹 Template Method

> Define el flujo general de ejecución del test, delegando pasos a subclases.

#### 🧪 Ejemplo

```ts
abstract class TestCase {
  run(result: TestResult) {
    this.setUp();
    try {
      (this as any)[this.name]();
    } catch {
      result.testFailed();
    }
    this.tearDown();
    result.testStarted();
  }

  setUp() {}
  tearDown() {}
}
```

Este patrón organiza y estandariza el ciclo de vida del test.

---

## Capítulo 30: Design Patterns

Este capítulo conecta el trabajo guiado por pruebas con patrones de diseño clásicos. Kent Beck muestra cómo estos patrones **emergen naturalmente** cuando se sigue el enfoque TDD. El propósito no es aplicarlos desde el principio, sino **reconocer cuándo aparecen como una solución limpia y necesaria**.

---

### 🔹 Command

> Encapsular una operación como un objeto.

Este patrón aparece cuando se quiere representar una acción (como `run`) como algo que puede pasarse, almacenarse o ejecutarse dinámicamente.

#### 🧪 Ejemplo en TypeScript

```ts
interface Command {
  execute(): void;
}

class PrintCommand implements Command {
  execute() {
    console.log("Print something");
  }
}
```

#### 🧠 En xUnit
El método `run()` de cada `TestCase` se comporta como un comando.

---

### 🔹 Value Object

> Objetos que se comparan por valor, no por identidad.

#### 🧪 Ejemplo

```ts
class Money {
  constructor(readonly amount: number, readonly currency: string) {}

  equals(other: Money): boolean {
    return this.amount === other.amount && this.currency === other.currency;
  }
}
```

TDD llevó a implementar `equals` para que los tests pudieran comparar resultados.

---

### 🔹 Null Object

> Reemplazar `null` por un objeto que tenga comportamiento neutral.

#### 🧠 Ejemplo conceptual

```ts
class NullLogger {
  log(msg: string) {} // No hace nada
}
```

#### 🧠 Beneficio
Permite evitar condiciones como `if (logger) logger.log(...)`.

---

### 🔹 Template Method

> Definir un algoritmo en una clase base y permitir que las subclases implementen los pasos.

#### 🧪 Ejemplo

```ts
abstract class TestCase {
  run(result: TestResult) {
    this.setUp();
    (this as any)[this.name]();
    this.tearDown();
    result.testStarted();
  }

  setUp() {}
  tearDown() {}
}
```

Este patrón define el ciclo de vida de un test.

---

### 🔹 Pluggable Object

> Pasar un objeto que encapsule lógica de test a quien lo ejecuta.

#### 🧪 Ejemplo

```ts
const suite = new TestSuite();
suite.add(new WasRun("testMethod"));
```

Permite desacoplar ejecución de definición.

---

### 🔹 Pluggable Selector

> Ejecutar un método por nombre, útil en frameworks dinámicos.

#### 🧪 Ejemplo

```ts
class TestCase {
  constructor(private name: string) {}
  run() {
    (this as any)[this.name]();
  }
}
```

Esto permite seleccionar dinámicamente qué test correr.

---

### 🔹 Factory Method

> Mover la lógica de creación a un método estático.

#### 🧪 Ejemplo

```ts
class Money {
  static dollar(amount: number): Money {
    return new Money(amount, "USD");
  }
}
```

Se usó para reducir duplicación y aclarar intención.

---

### 🔹 Imposter

> Objeto que reemplaza a otro para propósitos de prueba.

Difiere de Mock porque no está diseñado solo para verificación, sino para reemplazar funcionalidad real.

#### 🧠 Ejemplo

```ts
class FakeDB implements Database {
  // Implementación simple en memoria para tests
}
```

Esto apareció en el desarrollo de xUnit para reemplazar comportamiento temporalmente.

---

### 🔹 Composite

> Tratar objetos individuales y conjuntos de objetos de la misma forma.

#### 🧪 Ejemplo

```ts
interface Expression {
  reduce(bank: Bank, to: string): Money;
}

class Money implements Expression { ... }
class Sum implements Expression { ... }
```

Permite que `Money` y `Sum` se usen indistintamente como expresiones.

---

### 🔹 Collecting Parameter

> Pasar un objeto como parámetro y acumular resultados en él.

#### 🧠 Ejemplo

```ts
class TestResult {
  private failures = 0;
  testFailed() { this.failures++; }
}
```

TDD lo motivó para ir guardando el conteo de tests pasados/fallidos.

---

### 🔹 Singleton

> Un único punto de acceso a una instancia.

#### ⚠️ Beck lo menciona solo para **desaconsejar su uso**, a favor de pasar dependencias explícitamente.

```ts
// NO recomendado
const bank = Bank.getInstance();

// Recomendado
const bank = new Bank();
```

---

El diseño basado en TDD hace que los patrones **emerjan por necesidad**, no por anticipación. Reconocer estos patrones al momento justo ayuda a mantener el código limpio, expresivo y flexible. En lugar de forzar el uso de patrones, TDD los convierte en **herramientas de refactorización y claridad**, no en una meta en sí.

---

## Capítulo 31: Refactoring

Este capítulo presenta patrones concretos de refactorización, alineados con el enfoque TDD. A diferencia de la refactorización tradicional que busca no cambiar el comportamiento, en TDD lo que importa es **preservar los tests que pasan**, lo que puede permitir refactorizaciones más agresivas siempre y cuando las pruebas lo validen.

---

### 🔹 Reconcile Differences

> Unificá dos fragmentos de código similares, pero solo cuando sean **idénticos**.

#### 🧠 Estrategia
- No fuerces una generalización prematura.
- Andá paso a paso, volvé al refactor solo cuando la duplicación sea obvia.

#### 📌 Ejemplo
Si dos `if` tienen cuerpos similares, empezá por renombrar variables, luego mové el contenido a un método común.

---

### 🔹 Isolate Change

> Encapsulá la parte que necesitás modificar antes de tocarla.

#### 🧪 Ejemplo

```ts
function findRate(): number {
  // extraer lógica volátil aquí
  return this.internalRate();
}
```

Aislás para hacer cambios más seguros. Luego podés revertir si resulta innecesario.

---

### 🔹 Migrate Data

> Cambiá el formato de datos sin romper el código, manteniendo temporalmente ambos.

#### 🧠 Proceso

1. Agregá el nuevo campo.
2. Usá ambos campos en paralelo.
3. Eliminá el viejo campo cuando ya no se use.

#### 🧪 Ejemplo

```ts
class User {
  private legacyName?: string;
  private fullName: { first: string; last: string };

  constructor(first: string, last: string) {
    this.legacyName = `${first} ${last}`;
    this.fullName = { first, last };
  }
}
```

---

### 🔹 Extract Method

> Mové un fragmento de código a un nuevo método con un nombre descriptivo.

#### 🧪 Ejemplo

```ts
function printSummary() {
  this.printHeader();
  this.printDetails();
}

function printHeader() { ... }
function printDetails() { ... }
```

#### 💡 Usos
- Mejora la legibilidad.
- Reduce duplicación.

---

### 🔹 Inline Method

> Sustituí una llamada a método por el código real del método.

#### 🧪 Ejemplo

```ts
// en vez de:
return this.getPrice();

// usá:
return this.basePrice * 0.8;
```

Ayuda cuando el método extraído dejó de tener valor semántico.

---

### 🔹 Extract Object

> Mové parte de la responsabilidad de un objeto a uno nuevo.

#### 🧪 Ejemplo

```ts
class Address {
  constructor(public street: string, public city: string) {}
}

class User {
  address: Address;
}
```

Ideal cuando un objeto está creciendo demasiado.

---

### 🔹 Method Object

> Convertí un método complejo en una clase para facilitar la refactorización.

#### 🧠 Ventaja
Permite usar variables temporales y dividir en submétodos sin tener que pasar todos los parámetros.

---

### 🔹 Move Method

> Mové un método a otra clase donde tenga más sentido.

#### 🧠 Ejemplo típico

Si `getDiscount()` accede más a `Customer` que a su propia clase `Invoice`, considerá moverlo.

---

### 🔹 Add Parameter

> Agregá un parámetro a un método sin romper llamadas existentes (con soporte del compilador).

#### 🧪 Ejemplo

```ts
function calculate(price: number, taxRate?: number) {
  return price * (1 + (taxRate ?? 0.21));
}
```

Útil al extender comportamientos.

---

### 🔹 Method Parameter to Constructor Parameter

> Convertí un parámetro frecuente en una variable de instancia.

#### 🧠 Ejemplo

```ts
class Report {
  constructor(private format: string) {}

  print() {
    console.log(`Printing in ${this.format}`);
  }
}
```

Reduce duplicación y mejora claridad cuando el valor es común a varias llamadas.

---

Estas técnicas de refactorización son herramientas esenciales para mantener un diseño limpio, flexible y expresivo. En TDD, refactorizar es una actividad segura gracias al respaldo de los tests, y estas técnicas permiten hacerlo en pasos pequeños y controlados, sin romper el flujo de trabajo.


---

## Capítulo 32: Mastering TDD

Este capítulo final no introduce nuevos patrones o técnicas, sino que invita a la **reflexión profunda** sobre cómo aplicar, adaptar y escalar TDD. Kent Beck plantea una serie de preguntas abiertas con respuestas y comentarios que surgen de su experiencia, pensadas para quienes quieren llevar TDD más allá de una técnica básica.

---

### 🔸 ¿Cuánto feedback necesitás?

TDD acorta el ciclo de feedback entre diseño e implementación. En vez de esperar días o semanas para saber si tu diseño es bueno, escribís un test y lo comprobás en minutos.

> La cantidad y calidad del feedback definen la calidad de tus decisiones.

---

### 🔸 ¿Qué tan grandes deberían ser tus pasos?

- Tests grandes → más riesgo, menos validación inmediata.
- Tests pequeños → más seguridad, pero pueden sentirse lentos.

> Con la experiencia, los pasos tienden naturalmente a volverse más pequeños.

---

### 🔸 ¿Qué no necesitás testear?

“Escribí tests hasta que el miedo se transforme en aburrimiento.” – Phlip

#### 🧠 Solo necesitás testear:

- Condiciones que vos escribís.
- Lógica propia (no bibliotecas externas, salvo que desconfíes de ellas).
- Casos que aporten confianza o documentación.

---

### 🔸 ¿Cómo saber si tenés buenos tests?

Malas señales:

- Setup muy largo.
- Tests lentos.
- Tests frágiles que fallan con cambios no relacionados.
- Duplicación entre tests.

> Los tests deberían darte confianza, ser rápidos y claros.

---

### 🔸 ¿Podés aplicar TDD en sistemas grandes?

Sí, pero:

- Necesitás automatización y CI.
- Necesitás separar bien las responsabilidades.
- Los objetos pequeños y aislados hacen posible el testing en grande.

> Beck menciona un sistema de 250k líneas funcionales + 250k líneas de tests.

---

### 🔸 ¿Podés usar TDD con tests de nivel aplicación (ATDD)?

Sí, pero introduce retos:

- Técnicos: hacer fixtures sin que la funcionalidad exista.
- Sociales: los usuarios deben escribir (o al menos ayudar a escribir) tests.

> Requiere madurez del equipo. Primero dominá TDD a nivel programador.

---

### 🔸 ¿Cómo pasarse a TDD en un proyecto existente?

- No refactorices todo. Empezá donde tengas que cambiar algo.
- Aislá primero, testea después.
- Hacelo de a poco, estratégicamente.

> No podés testear un sistema entero de una vez. Usá el cambio como punto de entrada.

---

### 🔸 ¿Para quién es TDD?

No es para quien quiere “sólo hacer que funcione”. Es para quienes:

- Quieren aprender del diseño a través del código.
- Quieren mejorar continuamente.
- Valoran la elegancia como forma de productividad.

---

### 🔸 ¿Es TDD sensible al orden de los tests?

Sí. El orden puede hacer que ciertas secuencias sean mucho más fáciles de implementar.

> Como en la física: el comportamiento local puede ser caótico, pero el flujo general es predecible.

---

### 🔸 ¿Cómo se relaciona TDD con los patrones?

Los patrones emergen naturalmente cuando seguís reglas simples (como fake it, triangulación, implementación obvia).

> Los patrones no se aplican, **aparecen** cuando el código lo necesita.

---

### 🔸 ¿Por qué funciona TDD?

Beck plantea que es por la **reducción del ciclo de feedback**. Phlip sugiere que funciona como un “atractor” dinámico: si seguís las reglas (test, refactor, simplificá), el sistema converge hacia el orden.

> “Correctitud como función límite”.

---

### 🔸 ¿Cómo se relaciona con XP?

TDD potencia otras prácticas de XP:

- Pair programming: los tests son eje de discusión.
- Integración continua: tests permiten commits frecuentes.
- Diseño simple: codificás solo lo que el test pide.
- Refactor: los tests dan seguridad para mejorar el diseño.
- Entrega continua: más confianza, menos bugs en producción.

---

### 🧠 One Last Review

Tres descubrimientos clave al enseñar TDD:

1. Las tres formas de hacer que un test funcione: fake it, triangulate, obvious.
2. Eliminar duplicación como motor de diseño.
3. Controlar el espacio entre tests según la dificultad del terreno.

---

Este capítulo es una invitación a **hacer de TDD una práctica profesional reflexiva y adaptable**. No es un dogma, sino un conjunto de reglas simples que, si se aplican con consistencia y consciencia, llevan a un diseño más claro, más robusto y más disfrutable.