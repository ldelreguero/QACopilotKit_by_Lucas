---
name: qa-arquitecto
description: Diseña la arquitectura de automatización QA, configura proyectos Playwright Web/API, define estructura de carpetas, fixtures, convenciones y estrategia de pruebas. Usar cuando el usuario necesite setup, scaffolding, planificación o configuración técnica del framework de testing.

compatibility: Diseñada para GitHub Copilot en VS Code con proyectos Node.js y Playwright; opcionalmente integra Playwright MCP.

---

# QA arquitecto

## Cuándo usar

Usa esta skill cuando la tarea implique:

- crear un proyecto de automatización desde cero
- definir estructura Web, API o híbrida
- configurar Playwright, MCP, reporters, fixtures y variables de entorno
- establecer convenciones, carpetas y estrategia base de testing
- preparar el repositorio para que otros agentes implementen o ejecuten tests

No la uses como sustituto de planificación exploratoria interactiva de UI cuando el trabajo requiera recorrer una interfaz real y producir un plan táctico detallado.

## Resultado esperado

La salida debe dejar claro:

1. tipo de proyecto recomendado: web, api o hybrid
2. estructura de carpetas a crear
3. dependencias y configuración mínima necesarias
4. decisiones técnicas y tradeoffs
5. checklist de validación inicial

## Instrucciones

1. Clasifica el caso de uso.
   Decide si el proyecto es Web, API o Hybrid según el sistema bajo prueba y el alcance esperado.
2. Propón estructura mínima primero.
   Prioriza una base pequeña, extensible y mantenible antes de agregar complejidad.
3. Configura dependencias necesarias.
   Incluye solo paquetes justificados por el flujo real: Playwright, reporters, utilidades y MCP si aplica.
4. Define convenciones reutilizables.
   Establece ubicación de tests, fixtures, data, utils, reportes y variables de entorno.
5. Diseña la configuración de Playwright.
   Ajusta `testDir`, `projects`, `reporter`, `retries`, `workers`, `trace`, `screenshot` y `video` según el caso.
6. Evalúa MCP como opcional.
   Solo agréguelo si realmente se necesita inspección interactiva o debugging asistido del navegador.
7. Deja el proyecto listo para el siguiente paso.
   La salida debe permitir que un agente ejecutor cree tests sin redefinir la arquitectura.
8. Usa la base canónica para las reglas transversales.
   No reescribas estándares Playwright de locators, waits, POM o assertions si ya viven en `playwright-skill`.
9. Mantén la frontera con el planner interno.
   Si hace falta explorar una aplicación real para redactar escenarios detallados paso a paso, deriva al worker `playwright-test-planner` en vez de absorber ese flujo aquí.

## Recomendaciones de estructura

### Web

- `tests/ui/` para flujos de interfaz
- `fixtures/` para autenticación y estado compartido
- `utils/` para page objects y helpers
- `data/` para datos estáticos y ejemplos

### API

- `tests/api/contracts/` para contratos
- `tests/api/integration/` para flujos
- `tests/api/performance/` si hay mediciones básicas o smoke de performance
- `fixtures/` para API clients autenticados
- `data/schemas/` para contratos y payloads esperados

### Hybrid

- combina `tests/ui/`, `tests/api/` y `tests/hybrid/`
- reserva `tests/hybrid/` solo para flujos que realmente mezclen UI + API

## Dependencias recomendadas

- `@playwright/test`
- `dotenv`
- `prettier`
- `@faker-js/faker` cuando haya data dinámica justificada
- `allure-playwright` y `allure-commandline` solo si el equipo usa Allure
- `@playwright/mcp` y `@playwright/cli` solo si el flujo necesita inspección o asistencia interactiva

## Convenciones de configuración

- Usa `playwright.config.js` o `playwright.config.ts` consistente con el repo.
- Define `baseURL` y `API_BASE_URL` por variables de entorno.
- Activa `trace: 'on-first-retry'` y `screenshot: 'only-on-failure'` como punto de partida razonable.
- En CI, limita `workers` y habilita `retries` cuando el proyecto lo justifique.
- Evita meter secretos reales en `.env.example`.

## Criterios para habilitar MCP

Habilita MCP solo si necesitas:

- inspección visual rápida del DOM o accessibility tree
- debugging interactivo de un fallo difícil de reproducir
- generación asistida de selectores o validación manual del flujo

Si no existe esa necesidad, deja la configuración sin MCP para mantener menor complejidad operativa.

## Checklist inicial

- existe `package.json`
- Playwright está instalado
- el config carga sin errores
- las carpetas base coinciden con el tipo de proyecto
- existe `.env.example`
- los reporters elegidos están justificados
- MCP está configurado solo si es necesario

## Recursos relacionados

- [Blueprints de setup](references/blueprints-de-setup.md)
- [Arquitectura Playwright escalable](references/arquitectura-playwright-escalable.md)
- [Configuración y parametrización](references/configuracion-y-parametrizacion.md)
- [Tooling API condicional](references/tooling-api-condicional.md)
- [Playwright Skill](../playwright-skill/SKILL.md)
- [Test organization](../playwright-skill/core/test-organization.md)
- [api-suite-generation](../api-suite-generation/SKILL.md)
- [contract-first-api](../contract-first-api/SKILL.md)
- [openapi-testing](../openapi-testing/SKILL.md)
- [deployment-smoke-testing](../deployment-smoke-testing/SKILL.md)

## Casos borde

- Si el usuario pide setup híbrido pero no hay un caso real de UI + API combinados, recomienda empezar por Web o API puro.
- Si el repo ya tiene convenciones, respétalas antes de imponer una estructura nueva.
- Si falta contexto del sistema bajo prueba, prioriza un esqueleto mínimo y deja explícitas las decisiones pendientes.
- Si la estrategia ya está definida y solo falta bajar escenarios observando la UI, el siguiente paso correcto no es esta skill sino el planner interno especializado.