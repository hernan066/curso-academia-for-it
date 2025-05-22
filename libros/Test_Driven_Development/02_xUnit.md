# Test-Driven Development: By Example (Kent Beck) - Parte II: xUnit Example

En esta segunda parte del libro *Test-Driven Development: By Example*, Kent Beck nos guía en la creación de un **mini framework de testing**, inspirado en `xUnit`, usando el enfoque TDD. Aunque en el libro original se utiliza Python, aquí traducimos los conceptos y ejemplos a **TypeScript**.

El objetivo es mostrar cómo **una herramienta como un framework de testing puede diseñarse incrementalmente** a partir de pruebas automatizadas. Aprenderemos a construir componentes como `TestCase`, `TestResult` y `TestSuite`, y a aplicar patrones de diseño como el Template Method.

---

## Capítulo 18: WasRun

### 🎯 Objetivo
Crear una clase `WasRun` que simule una prueba ejecutada. Se busca validar si un método de prueba se ejecuta efectivamente.

### 🧪 Test

```ts
test("testMethod runs", () => {
  const test = new WasRun("testMethod");
  test.run();
  expect(test.wasRun).toBe(true);
});
```

### ✅ Implementación

```ts
class WasRun {
  wasRun = false;

  constructor(private name: string) {}

  run() {
    this.wasRun = true;
  }

  testMethod() {
    this.wasRun = true;
  }
}
```

> Por ahora, el método `run` simplemente marca `wasRun` como `true`, sin ejecutar el método por nombre. Es un primer paso.

---

## Capítulo 19: TestCase

### 🎯 Objetivo
Introducir una superclase `TestCase` que se encargue de la lógica de ejecución de pruebas.

### 🧪 Test

```ts
test("testMethod calls method dynamically", () => {
  const test = new WasRun("testMethod");
  test.run();
  expect(test.wasRun).toBe(true);
});
```

### ✅ Implementación de `TestCase` y refactor

```ts
class TestCase {
  constructor(private name: string) {}

  run() {
    const method = (this as any)[this.name];
    if (typeof method === "function") {
      method.call(this);
    }
  }
}

class WasRun extends TestCase {
  wasRun = false;

  constructor(name: string) {
    super(name);
  }

  testMethod() {
    this.wasRun = true;
  }
}
```

> Ahora el método `run` en `TestCase` llama dinámicamente al método de prueba por su nombre. `WasRun` ya no implementa `run`, sino que delega en `TestCase`.

---

## Capítulo 20: Template Method

### 🎯 Objetivo
Aplicar el patrón de diseño **Template Method**, que permite definir un algoritmo en una clase base, delegando pasos específicos a subclases.

### 🧪 Test

```ts
test("template method logs steps", () => {
  const test = new WasRun("testMethod");
  test.run();
  expect(test.log).toBe("setUp testMethod tearDown ");
});
```

### ✅ Implementación

```ts
class TestCase {
  constructor(protected name: string) {}

  run() {
    this.setUp();
    const method = (this as any)[this.name];
    if (typeof method === "function") {
      method.call(this);
    }
    this.tearDown();
  }

  setUp() {}
  tearDown() {}
}

class WasRun extends TestCase {
  log = "";

  setUp() {
    this.log += "setUp ";
  }

  testMethod() {
    this.log += "testMethod ";
  }

  tearDown() {
    this.log += "tearDown ";
  }
}
```

### ✅ Resultado
El patrón Template Method permite definir un flujo común (`setUp`, `test`, `tearDown`) en la clase base y dejar a las subclases la implementación concreta de cada paso.

---

## Capítulo 21: TestResult

### 🎯 Objetivo
Registrar cuántas pruebas pasaron y cuántas fallaron durante la ejecución.

### 🧪 Test

```ts
test("result summary", () => {
  const test = new WasRun("testMethod");
  const result = new TestResult();
  test.run(result);
  expect(result.summary()).toBe("1 run, 0 failed");
});
```

### ✅ Implementación

```ts
class TestResult {
  private runCount = 0;
  private failureCount = 0;

  testStarted() {
    this.runCount++;
  }

  testFailed() {
    this.failureCount++;
  }

  summary(): string {
    return `${this.runCount} run, ${this.failureCount} failed`;
  }
}
```

### 🛠 Refactor en `TestCase`

```ts
class TestCase {
  constructor(protected name: string) {}

  run(result: TestResult) {
    result.testStarted();
    this.setUp();
    try {
      const method = (this as any)[this.name];
      if (typeof method === "function") {
        method.call(this);
      }
    } catch {
      result.testFailed();
    }
    this.tearDown();
  }

  setUp() {}
  tearDown() {}
}
```

### ✅ Resultado
Ya podemos saber si una prueba se ejecutó y si falló, lo cual es esencial para tener confianza en nuestros tests.

---

## Capítulo 22: TestResult en acción

### 🎯 Objetivo
Asegurarse de que los resultados de las pruebas se reflejan correctamente cuando una prueba **falla intencionalmente**.

### 🧪 Test

```ts
class TestBroken extends TestCase {
  testBrokenMethod() {
    throw new Error("intentionally broken");
  }
}

test("failure result", () => {
  const test = new TestBroken("testBrokenMethod");
  const result = new TestResult();
  test.run(result);
  expect(result.summary()).toBe("1 run, 1 failed");
});
```

### ✅ Resultado
La clase `TestResult` detecta correctamente cuando una prueba falla. Gracias a esto, ahora el framework puede informar con precisión el estado de las ejecuciones.

---

Con estos capítulos, se completa una parte crítica del framework de testing: **la capacidad de reportar el resultado de las pruebas**. Esto es el núcleo de cualquier herramienta de testing moderna.

## Capítulo 23: TestCaseTest

### 🎯 Objetivo
Validar el comportamiento del framework construyendo una clase de prueba que lo testee. Esta clase representa un **caso de prueba para la propia clase `TestCase`**.

### 🧪 Test

```ts
class TestCaseTest extends TestCase {
  private result!: TestResult;

  setUp() {
    this.result = new TestResult();
  }

  testTemplateMethod() {
    const test = new WasRun("testMethod");
    test.run(this.result);
    expect(test.log).toBe("setUp testMethod tearDown ");
  }

  testResult() {
    const test = new WasRun("testMethod");
    test.run(this.result);
    expect(this.result.summary()).toBe("1 run, 0 failed");
  }

  testFailedResult() {
    const test = new WasRun("testBrokenMethod");
    test.run(this.result);
    expect(this.result.summary()).toBe("1 run, 1 failed");
  }
}
```

> `TestCaseTest` valida la ejecución completa del ciclo `setUp → test → tearDown` y verifica que los resultados se contabilicen correctamente.

---

## Capítulo 24: Ejecutar los tests

### 🎯 Objetivo
Permitir la ejecución manual de `TestCaseTest` y mostrar el resumen por consola.

### 🧪 Ejecución desde el programa principal

```ts
const result = new TestResult();

new TestCaseTest("testTemplateMethod").run(result);
new TestCaseTest("testResult").run(result);
new TestCaseTest("testFailedResult").run(result);

console.log(result.summary()); // Ejemplo: "3 run, 1 failed"
```

### ✅ Resultado
Ahora tenemos una forma de ejecutar automáticamente pruebas del framework, con un resumen útil del resultado. El sistema puede crecer y mantenerse con confianza gracias a su propia batería de tests.

---

Con estos capítulos, Kent Beck demuestra que incluso el propio framework puede y debe ser probado usando sus propias reglas. TDD se convierte así no solo en una técnica de desarrollo, sino en un enfoque **auto-sostenible y escalable**.

