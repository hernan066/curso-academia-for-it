# Test-Driven Development: By Example (Kent Beck) - Resumen en TypeScript

Este documento resume los primeros parte del libro _Test-Driven Development: By Example_ de Kent Beck, The Money Example , con ejemplos adaptados a TypeScript. El objetivo es entender cómo se aplica TDD en pasos pequeños, escribiendo primero las pruebas, luego el código mínimo necesario para que pasen, y finalmente refactorizando.

---

## Capítulo 1: Multi-Currency Money

### 🎯 Objetivo
Kent Beck inicia el proyecto creando una clase `Money` que representa una cantidad de dinero. El primer requerimiento es permitir la multiplicación de una cantidad por un número, por ejemplo:

> Si tengo `$5` y lo multiplico por `2`, quiero obtener `$10`.

Este capítulo introduce el ciclo básico de TDD: Red (test falla), Green (pasa), Refactor (mejoramos el diseño).

### 🧪 Test inicial

```ts
// money.test.ts
import { Money } from "./money";

test("multiplication", () => {
  const five = new Money(5, "USD");
  const result = five.times(2);
  expect(result.amount).toBe(10);
});
```

### ✅ Implementación mínima

```ts
// money.ts
export class Money {
  constructor(public amount: number, public currency: string) {}

  times(multiplier: number): Money {
    return new Money(this.amount * multiplier, this.currency);
  }
}
```

### 🧹 Resultado
Ya tenemos una clase `Money` con `amount` y `currency`, un método `times` que realiza la multiplicación, y una prueba que valida su comportamiento. Se establecen las bases del desarrollo iterativo con TDD.

---

## Capítulo 2: Degenerate Objects

### 🎯 Objetivo
Beck introduce intencionalmente **duplicación de lógica** para demostrar la importancia de escribir solo el código necesario para pasar los tests. Este enfoque, aunque "degenerado", fuerza a pensar en el diseño emergente.

> Primero hacerlo funcionar, luego mejorarlo.

### 🧪 Nuevos tests

```ts
test("multiplication by 2", () => {
  const five = new Money(5, "USD");
  const result = five.times(2);
  expect(result.amount).toBe(10);
});

test("multiplication by 3", () => {
  const five = new Money(5, "USD");
  const result = five.times(3);
  expect(result.amount).toBe(15);
});
```

### 🛠️ Implementación degenerada (temporal)

```ts
times(multiplier: number): Money {
  if (multiplier === 2) return new Money(10, this.currency);
  if (multiplier === 3) return new Money(15, this.currency);
  throw new Error("Unsupported multiplier");
}
```

### 🧹 Refactor posterior

Una vez que los tests están verdes, se generaliza la lógica:

```ts
times(multiplier: number): Money {
  return new Money(this.amount * multiplier, this.currency);
}
```

### ✅ Resultado
Este capítulo refuerza que no se debe escribir lógica genérica hasta que haya un test que lo requiera. Se destaca la disciplina de TDD en la toma de decisiones de diseño.

---

## Capítulo 3: Equality for All

### 🎯 Objetivo
Ahora se necesita comparar dos instancias de `Money` para verificar si representan el mismo valor y moneda.

> En JavaScript/TypeScript los objetos se comparan por referencia, no por valor.

Por eso, se implementa un método `equals` que evalúa si dos objetos `Money` son equivalentes.

### 🧪 Test

```ts
test("equality", () => {
  expect(new Money(5, "USD").equals(new Money(5, "USD"))).toBe(true);
  expect(new Money(5, "USD").equals(new Money(6, "USD"))).toBe(false);
});
```

### ✅ Implementación simple

```ts
equals(other: Money): boolean {
  return (
    this.amount === other.amount &&
    this.currency === other.currency
  );
}
```

### 💡 Mejora con verificación de tipo

```ts
equals(other: unknown): boolean {
  if (!(other instanceof Money)) return false;
  return this.amount === other.amount && this.currency === other.currency;
}
```

### ✅ Resultado
El diseño de objetos mejora con la capacidad de ser comparados por valor. Se mantiene el ciclo Red-Green-Refactor y se refuerza que los tests son los que guían la estructura del código.

---

Este enfoque paso a paso refleja la filosofía de Kent Beck: diseñar software limpio y flexible, guiado por pruebas automatizadas desde el inicio.

## Capítulo 4: Privacy

### 🎯 Objetivo
Este capítulo trata sobre **encapsulación**. Kent Beck destaca que las variables internas como `amount` deberían ser privadas, lo que refuerza la idea de ocultar la implementación y exponer solo lo necesario.

### 💡 Decisión de diseño
Aunque en el capítulo anterior accedimos directamente a `amount`, en este capítulo se reflexiona sobre mantener el encapsulamiento y usar métodos de acceso si es necesario.

### ✅ Código refactorizado en TypeScript

```ts
// money.ts
export class Money {
  #amount: number;
  #currency: string;

  constructor(amount: number, currency: string) {
    this.#amount = amount;
    this.#currency = currency;
  }

  times(multiplier: number): Money {
    return new Money(this.#amount * multiplier, this.#currency);
  }

  equals(other: unknown): boolean {
    if (!(other instanceof Money)) return false;
    return this.#amount === other.#amount && this.#currency === other.#currency;
  }

  getAmount(): number {
    return this.#amount;
  }

  getCurrency(): string {
    return this.#currency;
  }
}
```

> Se pasa a usar campos privados (`#amount`, `#currency`) y se agregan getters.

---

## Capítulo 5: Franc-ly Speaking

### 🎯 Objetivo
Introducir una nueva moneda: **francos suizos (CHF)**. Esto permite generalizar el diseño y asegurar que las operaciones no dependan solo de una moneda.

### 🧪 Nuevos tests

```ts
test("multiplication in CHF", () => {
  const five = new Money(5, "CHF");
  const result = five.times(2);
  expect(result.equals(new Money(10, "CHF"))).toBe(true);
});
```

### ✅ Resultado esperado

- El código actual ya soporta diferentes monedas porque `currency` es un atributo más, no hay lógica específica para USD.

> Este capítulo muestra cómo **los tests existentes validan la extensibilidad del diseño**. No hace falta cambiar la implementación si ya está bien estructurada.

---

## Capítulo 6: Equality Redux

### 🎯 Objetivo
Asegurarse de que dos objetos con **diferente moneda** no se consideren iguales aunque tengan el mismo monto.

### 🧪 Test adicional

```ts
test("different currencies not equal", () => {
  expect(new Money(5, "USD").equals(new Money(5, "CHF"))).toBe(false);
});
```

### ✅ Código ya preparado

Como en el método `equals` comparamos tanto el monto como la moneda, este test ya debería pasar.

```ts
equals(other: unknown): boolean {
  if (!(other instanceof Money)) return false;
  return this.#amount === other.#amount && this.#currency === other.#currency;
}
```

### ✅ Resultado
Este capítulo refuerza la confianza en el diseño ya existente, que pasa los nuevos tests sin necesidad de cambios. Eso demuestra que **el diseño orientado a pruebas permite mantener el código robusto y flexible**.

---

¿Sabías que en este punto ya estás haciendo diseño guiado por pruebas de forma natural? Cada nueva funcionalidad está impulsada por una prueba que guía su implementación y asegura su correcto funcionamiento.

---
## Capítulo 7: Apples and Oranges

### 🎯 Objetivo
Este capítulo plantea una prueba que compara objetos de diferentes clases para asegurarse de que `equals` no dé resultados incorrectos.

### 🧪 Test

```ts
test("comparison with different class returns false", () => {
  const money = new Money(5, "USD");
  const notMoney = { amount: 5, currency: "USD" };
  expect(money.equals(notMoney)).toBe(false);
});
```

### ✅ Implementación ya existente

Ya manejamos esto en la implementación previa:

```ts
equals(other: unknown): boolean {
  if (!(other instanceof Money)) return false;
  return this.#amount === other.#amount && this.#currency === other.#currency;
}
```

### ✅ Resultado
La clase `Money` se comporta correctamente al evitar comparar con objetos que no sean instancias de `Money`. Se refuerza la necesidad de verificar tipos al comparar.

---

## Capítulo 8: Makin' Objects

### 🎯 Objetivo
Reducir la duplicación al crear objetos `Money`. Kent Beck introduce métodos de fábrica como `Money.dollar(amount)` y `Money.franc(amount)`.

### 🧪 Nuevos métodos de creación

```ts
test("money factory methods", () => {
  const fiveDollars = Money.dollar(5);
  const fiveFrancs = Money.franc(5);
  expect(fiveDollars.equals(new Money(5, "USD"))).toBe(true);
  expect(fiveFrancs.equals(new Money(5, "CHF"))).toBe(true);
});
```

### ✅ Implementación

```ts
export class Money {
  // ...otros métodos y campos privados

  static dollar(amount: number): Money {
    return new Money(amount, "USD");
  }

  static franc(amount: number): Money {
    return new Money(amount, "CHF");
  }
}
```

### ✅ Resultado
Se mejora la legibilidad y se reduce el acoplamiento al usar métodos de fábrica. Esto hace que el código de test y producción sea más expresivo y claro.

---

## Capítulo 9: Times We Are Livin' In

### 🎯 Objetivo
Verificar que los métodos de fábrica no rompen la funcionalidad de multiplicación.

### 🧪 Test

```ts
test("multiplication using factory method", () => {
  const five = Money.dollar(5);
  const product = five.times(2);
  expect(product.equals(Money.dollar(10))).toBe(true);
});
```

### ✅ Resultado
El diseño orientado a pruebas nos permitió introducir un nuevo enfoque de creación (`dollar`, `franc`) sin romper la lógica existente. Todos los tests anteriores siguen funcionando.

---

Estos capítulos refuerzan cómo los tests te permiten modificar y mejorar el diseño sin miedo a romper funcionalidades previas, gracias al **feedback constante** de las pruebas automatizadas.

---

## Capítulo 10: Interesting Times

### 🎯 Objetivo
Hasta ahora, el método `times` siempre devolvía una instancia de `Money`. En este capítulo se introduce una nueva clase `Expression`, que permitirá representar operaciones más complejas (como sumas).

Se da un primer paso hacia una abstracción de operaciones.

### 🧪 Refactor: devolviendo `Expression`

```ts
interface Expression {}

export class Money implements Expression {
  // ...

  times(multiplier: number): Expression {
    return new Money(this.#amount * multiplier, this.#currency);
  }
}
```

### ✅ Resultado
Se implementa una interfaz `Expression` que `Money` implementa. Esto prepara el terreno para que otras clases, como `Sum`, también implementen esa interfaz.

---

## Capítulo 11: The Root of All Evil

### 🎯 Objetivo
Agregar la posibilidad de **sumar dos cantidades de dinero**, devolviendo una expresión que represente esa suma.

### 🧪 Test

```ts
test("simple addition returns Sum expression", () => {
  const five = Money.dollar(5);
  const result = five.plus(five);
  const sum = result as Sum;
  expect(sum.augend.equals(five)).toBe(true);
  expect(sum.addend.equals(five)).toBe(true);
});
```

### ✅ Implementación inicial

```ts
class Sum implements Expression {
  constructor(public augend: Money, public addend: Money) {}
}

export class Money implements Expression {
  // ...

  plus(addend: Money): Expression {
    return new Sum(this, addend);
  }
}
```

### ✅ Resultado
Se crea una clase `Sum` que representa la suma de dos objetos `Money`, y el método `plus` devuelve esa suma como una `Expression`. Aún no se puede evaluar el valor total, pero se sientan las bases.

---

## Capítulo 12: Addition, Finally

### 🎯 Objetivo
Evaluar el resultado de una suma. Para eso se introduce una nueva clase: `Bank`, que reduce una `Expression` a un `Money` concreto.

### 🧪 Test

```ts
test("reduce sum", () => {
  const sum = new Sum(Money.dollar(3), Money.dollar(4));
  const bank = new Bank();
  const result = bank.reduce(sum, "USD");
  expect(result.equals(Money.dollar(7))).toBe(true);
});
```

### ✅ Implementación

```ts
class Bank {
  reduce(source: Expression, to: string): Money {
    if (source instanceof Sum) {
      const amount = source.augend.getAmount() + source.addend.getAmount();
      return new Money(amount, to);
    }
    throw new Error("Unsupported expression");
  }
}
```

> También debemos hacer accesible `getAmount()` desde `Money` si usamos campos privados.

### ✅ Resultado
Ahora podemos representar y **evaluar sumas** de dinero usando una arquitectura orientada a expresiones. Esto abre la puerta a operaciones más complejas como conversión de monedas o cadenas de operaciones.

---

Con estos capítulos, Kent Beck comienza a abstraer operaciones financieras en estructuras reutilizables, manteniendo la simplicidad gracias al enfoque paso a paso de TDD.

---

## Capítulo 13: Make It

### 🎯 Objetivo
Agregar la capacidad al `Bank` de reducir directamente un objeto `Money`, no solo `Sum`.

### 🧪 Test

```ts
test("reduce money", () => {
  const bank = new Bank();
  const result = bank.reduce(Money.dollar(1), "USD");
  expect(result.equals(Money.dollar(1))).toBe(true);
});
```

### ✅ Implementación

```ts
class Bank {
  reduce(source: Expression, to: string): Money {
    if (source instanceof Sum) {
      const amount = source.augend.getAmount() + source.addend.getAmount();
      return new Money(amount, to);
    }
    if (source instanceof Money) {
      return source;
    }
    throw new Error("Unsupported expression");
  }
}
```

### ✅ Resultado
El `Bank` ahora puede reducir tanto `Money` como `Sum`, lo que amplía su utilidad para cualquier operación implementada hasta ahora.

---

## Capítulo 14: Money Redux

### 🎯 Objetivo
Revisar el diseño de `Money` para aplicar mejoras ahora que se tienen más requerimientos claros. Se busca **refactorizar sin cambiar el comportamiento**.

### 🔄 Cambios sugeridos

- Confirmar que `Money` también puede implementarse como `Expression` de manera más explícita.
- Asegurar que los métodos como `reduce` de `Bank` trabajen bien con cualquier `Expression`, sin crear código duplicado.

### ✅ Implementación (simplificada)

```ts
class Bank {
  reduce(source: Expression, to: string): Money {
    return source.reduce(this, to);
  }
}

interface Expression {
  reduce(bank: Bank, to: string): Money;
}

class Money implements Expression {
  // ...campos y constructor

  reduce(_bank: Bank, to: string): Money {
    return new Money(this.#amount, to);
  }
}

class Sum implements Expression {
  // ...campos y constructor

  reduce(bank: Bank, to: string): Money {
    const amount = this.augend.reduce(bank, to).getAmount() +
                   this.addend.reduce(bank, to).getAmount();
    return new Money(amount, to);
  }
}
```

### ✅ Resultado
Este capítulo refactoriza la arquitectura para que tanto `Money` como `Sum` se encarguen de su propia reducción. Esto mejora la extensibilidad del sistema.

---

## Capítulo 15: Equal Equals

### 🎯 Objetivo
Revisar nuevamente la lógica de igualdad, para asegurar que objetos con la **misma cantidad y moneda** sean iguales incluso después de reducirse.

### 🧪 Test

```ts
test("bank reduces identical money as equal", () => {
  const bank = new Bank();
  const result = bank.reduce(Money.dollar(5), "USD");
  expect(result.equals(Money.dollar(5))).toBe(true);
});
```

### ✅ Resultado
Este test confirma que la lógica de igualdad en `Money` es robusta incluso cuando los objetos se crean en diferentes contextos.

> Se refuerza el principio de que los objetos deben ser comparables por **valor**, no por identidad de instancia.

---

En estos capítulos, se consolida una estructura orientada a objetos más abstracta y escalable, con clases que se autogestionan (`Money` y `Sum`), y un `Bank` que actúa como intermediario para evaluar expresiones.

---

## Capítulo 16: Appendix - An End to End Example

### 🎯 Objetivo
Demostrar cómo se vería un caso de uso completo utilizando la infraestructura creada hasta el momento: sumar dos valores monetarios y reducir el resultado a una moneda.

### 🧪 Test de extremo a extremo

```ts
test("end-to-end addition and reduction", () => {
  const five = Money.dollar(5);
  const sum = five.plus(five); // retorna una expresión Sum
  const bank = new Bank();
  const reduced = bank.reduce(sum, "USD");
  expect(reduced.equals(Money.dollar(10))).toBe(true);
});
```

### ✅ Resultado
Se prueba la integración total entre `Money`, `Sum` y `Bank`. Esta prueba resume todos los pasos anteriores y verifica que el diseño soporta una operación realista de principio a fin.

> Este capítulo muestra la potencia de TDD: el sistema fue construido paso a paso, pero puede resolver casos completos sin necesidad de cambios.

---

## Capítulo 17: Why Make It?

### 🎯 Reflexión
Kent Beck hace una pausa para reflexionar sobre el propósito del enfoque que ha tomado: ¿por qué construir el sistema paso a paso, con duplicación temporal, pruebas triviales y refactor constante?

### 🔑 Ideas clave

- **Diseño guiado por uso:** Solo se escribe código que está respaldado por una prueba real.
- **Prevención del sobre-diseño:** Al evitar generalizaciones prematuras, se mantiene el sistema simple y enfocado.
- **Confianza:** Los tests permiten hacer cambios sin miedo, lo que da seguridad al refactorizar.
- **Diseño incremental:** El diseño emergente es sostenible y más flexible que uno planeado en su totalidad al principio.

### 🧠 Conclusión
El capítulo destaca que TDD no es solo una técnica de verificación, sino también una herramienta poderosa de **diseño iterativo y evolutivo**. El código que ha surgido es el reflejo directo de las necesidades expresadas en los tests.

---

Estos dos capítulos cierran la primera parte del libro con una demostración de que un enfoque riguroso y disciplinado puede generar código funcional, limpio y flexible desde cero.