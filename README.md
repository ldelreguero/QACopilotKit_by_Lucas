# templateAgentes

Este workspace está pensado para que un QA use agentes especializados de GitHub Copilot dentro de VS Code sin tener que decidir toda la arquitectura manualmente.

La idea general es simple:

1. Empiezas casi siempre por el orquestador QA.
2. El orquestador decide si conviene pasar al arquitecto, al ejecutor, al analista o a un worker interno de Playwright.
3. Tú te enfocas en el objetivo de testing, no en la mecánica interna de cada agente.

## Cómo empezar

Si no sabes qué agente usar, empieza por `@qa-maestro-orquestador`.

## Base canónica de Playwright

Las buenas prácticas transversales de Playwright en este workspace ya no salen de la skill anterior.
La fuente canónica actual es el repositorio `https://github.com/testdino-hq/playwright-skill`, incorporado en el repo como la skill `playwright-skill` bajo `.github/agents/skills/`.

Si necesitas reinstalarla o actualizarla en tu workspace, usa:

```bash
npx skills add testdino-hq/playwright-skill
```

Esto impacta a los agentes y skills QA que consumen estándares de:

- locators
- assertions y waiting
- test organization
- debugging
- page object model
- accesibilidad, visual testing, API testing y otros temas especializados

## Autoría y procedencia

Este workspace y marco de trabajo fue armado y adaptado por su autor para organizar el uso de agentes, skills, prompts e integración QA dentro de VS Code con GitHub Copilot.

### Datos del autor

- Autor: `Lucas del Reguero Martinez`
- LinkedIn: `https://www.linkedin.com/in/lucas-del-reguero-martinez/`

### Procedencia de componentes reutilizados

- La arquitectura general del workspace, la selección de agentes visibles, la orquestación QA, los prompts y la integración entre piezas fueron definidos y adaptados en este repositorio por su autor.
- Varias de las skills de este workspace fueron basadas o inspiradas en skills publicadas en `https://qaskills.sh/`, y luego adaptadas al contexto, estructura y criterios de este repositorio.
- La base canónica de buenas prácticas de Playwright proviene del repositorio `https://github.com/testdino-hq/playwright-skill` y se instala con `npx skills add testdino-hq/playwright-skill`.
- Los workers internos `playwright-test-planner`, `playwright-test-generator` y `playwright-test-healer` corresponden al flujo de agentes de Playwright documentado en `https://playwright.dev/docs/test-agents`.
- Esos workers fueron incorporados en este workspace como parte del sistema, pero su procedencia debe atribuirse a la documentación oficial de Playwright Test Agents.

El orquestador QA sigue siendo el punto de entrada recomendado porque:

- clasifica la necesidad
- decide si falta setup, ejecución o análisis
- reduce el riesgo de usar un agente demasiado específico demasiado pronto
- puede enrutar a workflows internos de Playwright cuando el caso lo requiere

Ejemplos de mensajes de inicio:

- `@qa-maestro-orquestador necesito crear tests para login y recuperación de contraseña`
- `@qa-maestro-orquestador tengo una suite fallando en Playwright, quiero encontrar la causa raíz`
- `@qa-maestro-orquestador necesito planificar la cobertura QA para esta nueva feature`
- `@qa-maestro-orquestador analiza estos resultados y dime si hay regresión`

## Orden recomendado de uso

Orden general para la mayoría de equipos QA:

1. `@qa-maestro-orquestador` si la necesidad mezcla varias fases o no tienes claro qué corresponde.
2. `@qa-arquitecto` si primero falta setup, estructura, estrategia o diseño del framework.
3. `@qa-ejecutor` si ya sabes qué test hay que escribir, correr o depurar.
4. `@qa-analista` si ya tienes resultados, reportes, respuestas API o artefactos para interpretar.
5. `@qa-seguridad` o `@qa-performance` cuando el objetivo ya es específicamente seguridad o performance.
6. `@qa-exploratorio` cuando quieres diseñar escenarios, charters o casos de prueba desde requerimientos, ejemplos o documentos.
7. `@Analista Universal de Logs` cuando el problema principal está en logs, trazas o correlación entre servicios.

## Agentes que puedes invocar directamente

Estos son los agentes pensados para uso directo por una persona QA:

### `@qa-maestro-orquestador`

Úsalo primero en caso de duda.

Características principales:

- es el punto de entrada recomendado
- enruta entre setup, ejecución, análisis, seguridad y performance
- decide cuándo usar workflows especializados de Playwright
- sirve bien para pedidos ambiguos o pipelines completos

Úsalo cuando:

- no tengas claro qué agente corresponde
- la tarea mezcle planificación, ejecución y análisis
- quieras que el sistema decida la mejor ruta

### `@qa-arquitecto`

Úsalo para preparar o rediseñar la base de automatización.

Características principales:

- configura proyectos Playwright
- define estructura de carpetas, fixtures y convenciones
- planifica estrategia de testing Web, API o híbrida
- decide si MCP vale la pena o no

Úsalo cuando:

- aún no existe una base de proyecto sólida
- necesitas setup de Playwright o MCP
- quieres definir estrategia, cobertura macro o diseño del framework

No es el agente correcto cuando lo que necesitas es recorrer una interfaz real y bajar un plan táctico paso a paso.

### `@qa-ejecutor`

Úsalo para trabajo operativo de testing.

Características principales:

- implementa tests manualmente
- ejecuta suites o archivos concretos
- depura fallos generalistas con Playwright MCP o CLI
- corrige selectores, waits, aserciones y problemas de flujo

Úsalo cuando:

- ya sabes qué test quieres escribir
- quieres correr tests y verificar resultado
- necesitas debuggear un fallo concreto sin entrar en un flujo interno especializado

### `@qa-analista`

Úsalo cuando el trabajo principal ya no es escribir tests sino interpretar resultados.

Características principales:

- analiza respuestas, artefactos y regresiones
- compara datos y JSON
- revisa resultados de ejecución y reportes
- ayuda a convertir evidencia técnica en veredicto accionable

Úsalo cuando:

- ya tienes salida de tests o reportes
- necesitas confirmar regresión o integridad de datos
- quieres comparar respuestas API o resultados históricos

### `@qa-seguridad`

Úsalo para validaciones de seguridad aplicativa.

Características principales:

- baseline de seguridad reproducible
- foco en OWASP, auth, permisos e inputs hostiles
- orientado a validaciones seguras y controladas

### `@qa-performance`

Úsalo para performance y carga.

Características principales:

- baseline de performance
- thresholds y degradación bajo carga
- perfiles de tráfico y lectura básica de métricas

### `@qa-exploratorio`

Úsalo para exploración y diseño de escenarios a partir de documentos, ejemplos o flujos poco definidos.

Características principales:

- análisis de requerimientos
- diseño de casos de prueba
- enfoque exploratorio antes de automatizar

### `@Analista Universal de Logs`

Úsalo cuando el insumo principal son logs, stack traces o eventos distribuidos.

Características principales:

- análisis técnico de logs
- identificación de causa raíz
- correlación entre servicios
- salida técnica o ejecutiva según necesidad

## Agentes internos que no debes invocar directamente

Estos agentes existen, pero están pensados para uso interno del sistema. No son la puerta principal para el usuario final.

### `playwright-test-planner`

Es un worker interno.

Qué hace:

- explora una UI real en navegador
- arma un plan táctico detallado
- prepara escenarios que luego pueden convertirse en tests

Quién lo activa:

- normalmente `@qa-maestro-orquestador`

### `playwright-test-generator`

Es un worker interno.

Qué hace:

- toma un plan existente
- ejecuta pasos reales
- genera un archivo de test Playwright consistente con ese plan

Quién lo activa:

- normalmente `@qa-maestro-orquestador`

### `playwright-test-healer`

Es un worker interno.

Qué hace:

- ejecuta tests fallidos
- entra en depuración iterativa
- corrige y revalida hasta recuperar el test o documentar el bloqueo

Quién lo activa:

- normalmente `@qa-maestro-orquestador`

## Qué agente usar en cada situación

| Necesidad del QA | Agente recomendado |
| --- | --- |
| No sé por dónde empezar | `@qa-maestro-orquestador` |
| Necesito setup o estructura del proyecto | `@qa-arquitecto` |
| Necesito estrategia o cobertura macro | `@qa-arquitecto` |
| Quiero escribir o correr tests Playwright | `@qa-ejecutor` |
| Tengo resultados y quiero analizarlos | `@qa-analista` |
| Necesito validar seguridad | `@qa-seguridad` |
| Necesito validar performance o carga | `@qa-performance` |
| Quiero diseñar casos desde requerimientos | `@qa-exploratorio` |
| Tengo logs y necesito causa raíz | `@Analista Universal de Logs` |

## Flujo recomendado para un QA

### Caso 1: proyecto nuevo o feature nueva

1. Empieza con `@qa-maestro-orquestador`.
2. Si falta base técnica, te llevará a `@qa-arquitecto`.
3. Después, el trabajo operativo caerá en `@qa-ejecutor`.
4. Si necesitas interpretar resultados, termina en `@qa-analista`.

### Caso 2: suite Playwright fallando

1. Empieza con `@qa-maestro-orquestador`.
2. Si el caso es simple, puede llevarte a `@qa-ejecutor`.
3. Si la recuperación exige un ciclo intensivo de run, debug y fix, activará el worker interno `playwright-test-healer`.

### Caso 3: necesito un plan antes de automatizar

1. Si es estrategia general, usa `@qa-arquitecto`.
2. Si quieres explorar una UI real y bajar pasos concretos, entra por `@qa-maestro-orquestador`; internamente puede usar `playwright-test-planner`.

### Caso 4: necesito análisis de evidencia

1. Para resultados QA, usa `@qa-analista`.
2. Para logs y stack traces, usa `@Analista Universal de Logs`.

## Prompts útiles

Además de invocar agentes directamente, el repo incluye prompts reutilizables en `.github/prompts/`.

Prompts QA disponibles:

- `qa-maestro-orquestador`: clasifica una necesidad QA y decide el siguiente agente
- `qa-arquitecto`: pide setup o diseño de framework QA
- `qa-ejecutor`: implementa, ejecuta o depura tests Playwright
- `qa-analista`: analiza artefactos, respuestas o reportes

Prompts de logs disponibles:

- `analisis-base-logs`
- `comparacion-multiples-logs`
- `identificacion-causa-raiz`
- `resumen-ejecutivo`

## Qué hace especial a este workspace

- Está orientado a QA real, no solo a creación de agentes.
- El marco completo del workspace fue armado y adaptado en este repositorio para uso QA sobre VS Code y GitHub Copilot.
- La capa Playwright usa `playwright-skill` como base canónica común para evitar duplicación de criterios.
- Esa base proviene de `testdino-hq/playwright-skill` y puede reinstalarse con `npx skills add testdino-hq/playwright-skill`.
- Los workers `playwright-test-planner`, `playwright-test-generator` y `playwright-test-healer` siguen la instalación y enfoque documentados por Playwright en `https://playwright.dev/docs/test-agents`.
- Los workflows más complejos están separados en workers internos para que el usuario no tenga que elegir entre agentes demasiado parecidos.
- El orquestador QA funciona como puerta de entrada recomendada.

## Recomendación práctica final

Si eres QA y solo quieres empezar a trabajar, usa esta regla:

1. Empieza por `@qa-maestro-orquestador`.
2. Ve directo a otro agente solo si ya sabes exactamente que necesitas setup, ejecución, análisis, seguridad, performance, exploración o análisis de logs.
3. No intentes invocar los workers internos de Playwright; están para que el sistema los use por ti cuando haga falta.
