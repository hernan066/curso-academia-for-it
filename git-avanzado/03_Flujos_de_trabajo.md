# Flujos de Trabajo

En este documento se resumen y comparan dos de los flujos de trabajo más populares en Git: **GitFlow** y **Trunk-Based Development**, según lo explicado en un video informativo.

---

## 🔀 1. GitFlow

**Propuesto por**: Vincent Driessen (2010)  
**Estructura principal**:
- `main`: rama de producción. Siempre estable.
- `develop`: integración de funcionalidades. Semi-estable.
- `feature/*`: ramas para desarrollar funcionalidades.
- `release/*`: ramas de preparación para producción.
- `hotfix/*`: ramas para corregir problemas urgentes.

### 🧭 Flujo típico

1. Crear una rama `feature/` desde `develop`.
2. Merge a `develop` mediante pull request.
3. Crear `release/` desde `develop` cuando se quiere hacer un deploy.
4. Merge de `release/` a `main` y también a `develop`.
5. Crear `hotfix/` desde `main` si hay bugs urgentes.

### ✅ Ventajas

- Buena separación de ambientes.
- Útil para ciclos de desarrollo largos y versiones planificadas.

### ❌ Desventajas

- Complejo, muchas ramas a mantener.
- Riesgo de conflictos y ramas desincronizadas.
- Obstaculiza CI/CD.
- Mucho esfuerzo para seguir reglas estrictas.

---

## 🌳 2. Trunk-Based Development

**Difusión por**: Paul Hammond y comunidad DevOps  
**Estructura principal**:
- Solo una rama principal: `main` o `trunk`.

### 🧭 Flujo típico

1. Crear ramas `feature` de corta duración desde `main`.
2. Merge a `main` mediante pull request en 1–2 días.
3. CI ejecuta tests automáticos para validar.
4. Los releases se marcan con tags en `main`.
5. Los hotfix también se hacen desde `main`.

### 🛠️ Prácticas comunes

- **Feature Flags**
- **Branch by Abstraction**

### ✅ Ventajas

- Simple y fácil de entender.
- Cambios pequeños y frecuentes.
- Compatible con CI/CD.
- Menos conflictos.
- Fomenta colaboración en el equipo.

### ❌ Desventajas

- Requiere buena disciplina de testing e integración.

---

## 📊 Comparación

| Característica             | GitFlow                  | Trunk-Based Development    |
|---------------------------|--------------------------|----------------------------|
| Complejidad                | Alta                     | Baja                       |
| Ideal para                | Versiones planificadas   | Despliegue continuo        |
| Nº de ramas activas       | Muchas                   | Pocas                      |
| CI/CD                     | Difícil de integrar      | Naturalmente compatible    |
| Tiempo de integración     | Semanas                  | Horas o días               |
| Resolución de conflictos  | Frecuente                | Poco frecuente             |

---

## 🧠 Actividades

### 1. Git Flow en Proyectos Modernos

**Pregunta**:  
El video critica Git Flow por ser complejo y burocrático. Investiga proyectos de software de código abierto o empresas que aún utilizan Git Flow. Analiza las razones por las que podrían preferir este flujo de trabajo, considerando el tamaño del equipo, la complejidad del proyecto y los requisitos de cumplimiento normativo. ¿En qué escenarios específicos crees que Git Flow podría seguir siendo una opción viable?

**Respuesta**:

Aunque Git Flow ha perdido popularidad frente a modelos como Trunk-Based Development, aún se utiliza en ciertos contextos donde la estabilidad, trazabilidad y planificación estructurada son prioritarias.

#### 🏢 Ejemplos de uso real

- **Software Empresarial (ERP, sistemas bancarios)**: donde los lanzamientos son trimestrales y altamente regulados.
- **Proyectos Open Source como Symfony o Jenkins**: requieren separar funcionalidades nuevas de versiones estables.
- **Empresas con entornos regulados**: por ejemplo, Fintech o Salud que necesitan cumplir normativas como ISO, HIPAA o PCI-DSS.

#### ✅ Razones para usar Git Flow

| Factor                             | Justificación                                                   |
|------------------------------------|-----------------------------------------------------------------|
| Tamaño del equipo                  | Equipos grandes se benefician de ramas organizadas.             |
| Ciclos de release predecibles      | Permite estabilizar antes de producción.                        |
| Cumplimiento normativo o auditoría | Permite demostrar cuándo y cómo se hicieron cambios.            |
| Ambientes múltiples                | Encaja bien con `develop`, `staging`, `production`.             |
| Clientes conservadores             | Algunos requieren procesos controlados y trazables.             |

#### 🧩 Escenarios viables para Git Flow

1. **Proyectos con versiones LTS**: donde se necesita mantener y parchear versiones en paralelo.
2. **Sistemas embebidos o firmware**: pruebas extensas y deployments controlados.
3. **Entornos altamente regulados**: que exigen trazabilidad detallada.
4. **Agencias con múltiples clientes**: ramas `release/cliente-X` permiten aislar cambios.

---

### 2. Estrategias de Feature Flags en TypeScript

**Pregunta**:  
El video menciona "Feature Flags" como una técnica clave para el desarrollo basado en Trunk. Investiga cómo se implementan los Feature Flags en proyectos.

Una vez hayas visto el módulo de TypeScript: Explora diferentes librerías o patrones de diseño que faciliten la gestión de Feature Flags. Crea un ejemplo simple de un Feature Flag en TypeScript que permita habilitar o deshabilitar una funcionalidad específica en tu código. ¿Cómo te parece que se integrarían los Feature Flags en un flujo de trabajo de Trunk-based Development?


**Respuesta**:  
Los Feature Flags (o banderas de funcionalidad) son una técnica que permite habilitar o deshabilitar funcionalidades específicas en tiempo de ejecución, sin necesidad de modificar ni volver a desplegar el código. Esto resulta especialmente útil para habilitar funcionalidades solo para ciertos entornos, usuarios o fases de desarrollo.

---

### Beneficios clave

- ✅ Activación controlada de features (por entorno, usuario o tiempo)
- ✅ Habilita despliegues continuos sin exposición inmediata
- ✅ Facilita el testing A/B o canary releases
- ✅ Permite hacer rollback rápido sin revertir código

---

### Librerías y patrones en TypeScript

A continuación, se listan algunas librerías y enfoques populares para implementar Feature Flags en proyectos TypeScript:

| Herramienta / Patrón     | Descripción breve |
|--------------------------|-------------------|
| [`unleash-client`](https://github.com/Unleash/unleash-client-node) | Cliente oficial de Unleash, ideal para gestión de flags centralizada |
| `splitio`, `flagsmith`   | SaaS orientados a AB testing y activación por segmento |
| `fflip`                  | Librería simple basada en archivos de configuración locales |
| 🧱 Patrón artesanal       | Configuración local con objeto de flags y funciones de acceso |

---

### Ejemplo simple de Feature Flag en TypeScript

#### `featureFlags.ts`
```ts
export const featureFlags = {
  enableNewCheckout: true,
  enableBetaSearch: false,
};
```

#### `features.ts`
```ts
import { featureFlags } from './featureFlags';

export function isFeatureEnabled(name: keyof typeof featureFlags): boolean {
  return featureFlags[name] === true;
}
```

#### `checkout.ts`
```ts
import { isFeatureEnabled } from './features';

export function renderCheckout() {
  if (isFeatureEnabled('enableNewCheckout')) {
    return renderNewCheckout();
  } else {
    return renderOldCheckout();
  }
}
```

---

### Integración con Trunk-Based Development

El enfoque Trunk-Based Development (TBD) implica que todo el equipo trabaja sobre una única rama principal (`main`) y realiza despliegues frecuentes.

Los Feature Flags permiten:

- 💡 **Mergear código incompleto sin riesgo:** al estar “escondido” tras una bandera
- 🧪 **Habilitar features solo en staging o QA**
- 🔒 **Activar funcionalidades solo para usuarios internos o testers**
- 🔄 **Hacer rollback rápido sin revertir merges**

#### Flujo sugerido con Feature Flags en TBD

1. Implementar la nueva funcionalidad detrás de un flag.
2. Mergear a `main` aunque el feature no esté listo.
3. Activar la bandera cuando el feature esté validado.
4. Eliminar el flag una vez estabilizado el feature (limpieza técnica).


---

### 3. Branch by Abstraction

**Pregunta**:  
Además de Feature Flags, el video introduce "Branch by Abstraction". Investiga a fondo esta técnica. Describí en tus propias palabras cómo funciona.

Una vez hayas visto el módulo de TypeScript, busca ejemplos prácticos de cómo se puede aplicar Branch by Abstraction en proyectos TypeScript para refactorizar código o introducir nuevas funcionalidades sin interrumpir el flujo de trabajo principal.


**Respuesta**:  
**Branch by Abstraction** es una técnica de desarrollo que permite realizar grandes cambios en el código (como refactorizaciones o nuevas funcionalidades) **sin crear ramas separadas**. En lugar de trabajar en un feature branch largo y riesgoso, se introduce una **abstracción intermedia** que permite que el código viejo y nuevo convivan temporalmente dentro de la misma base de código.

---

### 🧠 ¿Cómo funciona?

1. **Crear una interfaz o clase abstracta** que represente la funcionalidad existente.
2. **Encapsular la implementación actual** detrás de esa abstracción.
3. Implementar una **nueva versión** que también cumpla con esa interfaz.
4. Permitir que el sistema elija cuál usar (mediante un feature flag, configuración o inyección).
5. Una vez validada la nueva implementación, eliminar la abstracción temporal y el código viejo.

---

### ✅ Ventajas

- Evita conflictos al integrar cambios grandes.
- Permite realizar **refactorizaciones incrementales**.
- Facilita el testing y validación en producción.
- Compatible con **Trunk-Based Development** y entrega continua.

---

### 🧪 Ejemplo práctico en TypeScript

#### Paso 1: Crear una interfaz

```ts
// interfaces/NotificationService.ts
export interface NotificationService {
  send(to: string, message: string): Promise<void>;
}
```

#### Paso 2: Adaptar la implementación actual

```ts
// services/EmailNotification.ts
import { NotificationService } from '../interfaces/NotificationService';

export class EmailNotification implements NotificationService {
  async send(to: string, message: string) {
    console.log(`Enviando email a ${to}: ${message}`);
  }
}
```

#### Paso 3: Crear nueva implementación

```ts
// services/PushNotification.ts
import { NotificationService } from '../interfaces/NotificationService';

export class PushNotification implements NotificationService {
  async send(to: string, message: string) {
    console.log(`Enviando push a ${to}: ${message}`);
  }
}
```

#### Paso 4: Selección en tiempo de ejecución (por config o flag)

```ts
import { NotificationService } from './interfaces/NotificationService';
import { EmailNotification } from './services/EmailNotification';
import { PushNotification } from './services/PushNotification';

const usePush = process.env.FEATURE_PUSH_NOTIFICATIONS === 'true';

const notificationService: NotificationService = usePush
  ? new PushNotification()
  : new EmailNotification();
```

---

### 🚀 Aplicación en Trunk-Based Development

Con Branch by Abstraction:

- El equipo puede **mergear cambios incompletos sin romper `main`**.
- La nueva funcionalidad se activa gradualmente.
- No se necesitan ramas largas ni complejas.
- El cambio es reversible fácilmente.

---

### 4. Integración Continua

**Pregunta**:  
El video destaca la importancia de la Integración Continua (CI) en Trunk-based Development. Investiga cómo se configura un pipeline de CI para un proyecto de git usando alguna de las herramientas más populares como GitHub Actions, CircleCI o GitLab CI.

Una vez hayas visto el módulo de Testing Automatizado, crea un ejemplo de un pipeline de CI que incluya pruebas unitarias y de integración. ¿Cómo se relaciona este pipeline con el concepto de Trunk-based Development? ¿Qué beneficios aporta a la calidad del código y la velocidad de desarrollo?


**Respuesta**:  
La **Integración Continua (CI)** es una práctica fundamental en Trunk-Based Development. Consiste en integrar cambios en la rama principal (`main`) varias veces al día, ejecutando automáticamente una **suite de pruebas y validaciones** para detectar errores temprano.

---

### 🛠️ Herramientas comunes para CI

- **GitHub Actions** (nativo en GitHub)
- **GitLab CI/CD**
- **CircleCI**
- **Travis CI**
- **Jenkins**

Cada una permite configurar *pipelines* automatizados que se ejecutan ante eventos como *push*, *pull request* o *merge*.

---

### 🧪 Ejemplo: Pipeline de CI con GitHub Actions para TypeScript

Este ejemplo corre **tests unitarios y de integración** usando `Vitest` y `ts-node`.

#### `.github/workflows/ci.yml`
```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build-and-test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout código
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Instalar dependencias
        run: npm install

      - name: Ejecutar tests unitarios y de integración
        run: npm run test
```

#### `package.json`
```json
{
  "scripts": {
    "test": "vitest run"
  }
}
```

---

### 🤝 Relación con Trunk-Based Development

La CI es el **pegamento técnico** de Trunk-Based Development:

- 🧪 **Valida cada cambio en `main` automáticamente**
- 🚫 **Previene la rotura del código compartido**
- ⚡ **Permite detectar errores de inmediato**
- ✅ **Habilita despliegues continuos con confianza**
- 🔄 **Favorece ciclos de feedback cortos**

---

### ✅ Beneficios para el equipo

| Beneficio                       | Impacto |
|----------------------------------|---------|
| Detección temprana de errores    | Menor costo de corrección |
| Validación automática del código | Ahorra tiempo de revisión |
| Flujo estable de integración     | Menos conflictos entre ramas |
| Confianza para refactorizar      | Seguridad al modificar código |
| Entregas rápidas y seguras       | Aceleración del ciclo Dev→Prod |

---

### 5. Conflictos de Merge en Trunk-based Development

**Pregunta**:  
El video afirma que Trunk-based Development minimiza los conflictos de merge. Sin embargo, estos conflictos pueden seguir ocurriendo. Investiga estrategias para prevenir conflictos de merge en un entorno de Trunk-based Development. ¿Qué buenas prácticas de desarrollo (por ejemplo, comunicación frecuente, revisiones de código exhaustivas) ayudan a evitar conflictos? Si ocurre un conflicto, ¿cuál es el proceso recomendado para resolverlo de manera rápida y eficiente?


**Respuesta**:  
Aunque **Trunk-Based Development** minimiza los conflictos al integrar cambios frecuentemente en `main`, los conflictos de merge aún pueden ocurrir cuando:
- Múltiples personas editan los mismos archivos o líneas.
- Hay refactorizaciones estructurales concurrentes.
- No hay suficiente coordinación entre desarrolladores.

---

### 🛡️ Estrategias para prevenir conflictos de merge

#### 1. 🔁 **Integración frecuente**
- Hacer `merge` o `rebase` contra `main` varias veces al día.
- Evitar ramas largas que divergen por días o semanas.

#### 2. 🧭 **Feature Flags y Branch by Abstraction**
- Permiten integrar trabajo incompleto sin interferir con código existente.
- Evitan tener que aislar grandes cambios en ramas prolongadas.

#### 3. 🧩 **Dividir cambios grandes en pequeños commits**
- Cambios pequeños y atómicos son más fáciles de revisar y fusionar.

#### 4. 💬 **Comunicación continua**
- Hablar con tu equipo antes de tocar módulos compartidos o sensibles.
- Coordinar refactorizaciones o migraciones masivas.

#### 5. 🔍 **Revisiones de código rápidas y eficientes**
- Promueven integración temprana de feedback.
- Detectan cambios solapados antes de que escalen.

---

### ⚔️ ¿Qué hacer cuando ocurre un conflicto?

#### ✅ Proceso recomendado:

1. **Identificá los archivos en conflicto** con `git status`.
2. **Revisá el contenido de cada conflicto** con herramientas visuales (`git mergetool`, VSCode, etc).
3. **Charlá con el otro dev si es necesario** para entender la intención.
4. **Probá y corré los tests después de resolver**.
5. **Hacé commit de la resolución y seguí trabajando normalmente**.

---

### 🧠 Buenas prácticas extra

- Evitá renombrar archivos o mover carpetas sin comunicarlo.
- Usá herramientas de linters o formatters (como Prettier) para reducir conflictos por estilo.
- Automatizá CI para detectar errores inmediatamente después de resolver conflictos.











