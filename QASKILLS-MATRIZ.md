# Matriz de Disposición de qaskills

Esta matriz resume el estado final de consolidación del árbol legacy `qaskills` hacia la capa activa del repositorio.

## Criterios usados

- `Duplicada`: ya existe cobertura suficiente en una skill activa o en `playwright-best-practices`.
- `Parcial`: comparte dominio con un agente actual, pero aporta subtemas o material reutilizable.
- `No relacionada`: no encaja bien en la topología actual y apunta a un nuevo dominio operativo.

## Acciones posibles

- `Fusionar`: extraer deltas útiles en español e integrarlos en una skill activa o en `references/`.
- `Nueva skill`: crear una skill temática bajo un agente existente.
- `Integrado`: el contenido ya fue absorbido por una skill activa, nueva o existente.
- `Nuevo agente + skill`: abrir un nuevo dominio operativo con agente propio.
- `Evaluar`: dejar en observación hasta revisar el cluster completo o confirmar stack real del equipo.

## Prioridad de ejecución por cluster

1. Playwright, visual, accessibility y debugging.
2. API, contratos, OpenAPI, reporting y observabilidad.
3. Seguridad.
4. Performance y carga.
5. Tooling específico.

## Matriz completa

| qaskill legacy | Cluster | Agente relacionado | Skill activa relacionada | Duplicidad | Acción recomendada | Destino propuesto |
| --- | --- | --- | --- | --- | --- | --- |
| ab-testing-validation | experimentación | ninguno claro | ninguna | no relacionada | Evaluar | permanece fuera del árbol activo salvo adopción explícita de experimentación |
| accessibility-auditor | accessibility | `qa-ejecutor` | `playwright-accessibility` | parcial | Integrado | `playwright-accessibility` |
| agent-browser | browser-tooling | ninguno claro | ninguna | no relacionada | Evaluar | permanece fuera del árbol activo salvo adopción explícita de `agent-browser` |
| allure-report-generator | reporting | `qa-analista` | `qa-analista` | parcial | Integrado | reference `allure-y-tendencias.md` en `qa-analista` |
| api-contract-validator | contratos API | `qa-analista` | `qa-analista` | parcial | Integrado | reference `validacion-de-contratos-api.md` en `qa-analista` |
| api-test-suite-generator | API/OpenAPI | `qa-arquitecto` | `api-suite-generation` | parcial | Integrado | `api-suite-generation` |
| api-testing-rest | API REST | `qa-ejecutor` | `playwright-api` | parcial | Integrado | `playwright-api` |
| artillery-load-testing | performance | `qa-performance` | `qa-performance` | parcial | Integrado | reference `artillery-para-carga-declarativa.md` en `qa-performance` |
| artillery-load | performance | `qa-performance` | `qa-performance` | parcial | Integrado | reference `artillery-para-carga-declarativa.md` en `qa-performance` |
| audit-website | auditoría web | ninguno claro | ninguna | no relacionada | Evaluar | permanece fuera del árbol activo; requiere una decisión específica de auditoría transversal |
| auth-bypass-tester | seguridad | `qa-seguridad` | `qa-seguridad` | parcial | Integrado | reference `auth-bypass-y-sesion.md` en `qa-seguridad` |
| configuration-testing | estrategia/calidad | `qa-arquitecto` | `qa-arquitecto` | parcial | Integrado | reference `configuracion-y-parametrizacion.md` en `qa-arquitecto` |
| console-error-hunter | debugging | `qa-ejecutor` | `qa-ejecutor` | parcial | Integrado | reference `playwright-operativo-avanzado.md` en `qa-ejecutor` |
| contract-first-testing | contratos API | `qa-arquitecto` | `contract-first-api` | parcial | Integrado | `contract-first-api` |
| contract-test-generator | contratos API | `qa-arquitecto` | `contract-first-api` | parcial | Integrado | `contract-first-api` |
| e2e-testing-patterns | Playwright E2E | `qa-ejecutor` | `qa-ejecutor` | duplicada | Integrado | reference `playwright-operativo-avanzado.md` en `qa-ejecutor` |
| k6-performance | performance | `qa-performance` | `qa-performance` | parcial | Integrado | reference `baseline-k6-reproducible.md` en `qa-performance` |
| llm-security-testing | seguridad | `qa-seguridad` | `qa-seguridad` | parcial | Integrado | reference `seguridad-base-para-apps-llm.md` en `qa-seguridad` |
| openapi-test-generation | API/OpenAPI | `qa-arquitecto` | `openapi-testing` | parcial | Integrado | `openapi-testing` |
| opentelemetry-testing | observabilidad | `qa-analista` | `observability-analysis` | parcial | Integrado | `observability-analysis` |
| owasp-security | seguridad | `qa-seguridad` | `qa-seguridad` | parcial | Integrado | reference `baseline-owasp-reproducible.md` en `qa-seguridad` |
| pairwise-test-generator | diseño de pruebas | `qa-exploratorio` | `pairwise-test-design` | parcial | Integrado | `pairwise-test-design` |
| playwright-8-layer-test-architecture | arquitectura Playwright | `qa-arquitecto` | `qa-arquitecto` | parcial | Integrado | reference `arquitectura-playwright-escalable.md` en `qa-arquitecto` |
| playwright-advance-e2e | Playwright E2E | `qa-ejecutor` | `qa-ejecutor` | parcial | Integrado | reference `playwright-operativo-avanzado.md` en `qa-ejecutor` |
| playwright-api | Playwright API | `qa-ejecutor` | `playwright-api` | parcial | Integrado | `playwright-api` |
| playwright-cli-automation | CLI Playwright | `qa-ejecutor` | `qa-ejecutor` | parcial | Integrado | reference `playwright-operativo-avanzado.md` en `qa-ejecutor` |
| playwright-e2e | Playwright E2E | `qa-ejecutor` | `qa-ejecutor` | duplicada | Integrado | reference `playwright-operativo-avanzado.md` en `qa-ejecutor` |
| playwright-network-testing | network/debugging | `qa-ejecutor` | `qa-ejecutor` | parcial | Integrado | reference `playwright-operativo-avanzado.md` en `qa-ejecutor` |
| playwright-skill-enhanced | Playwright general | `qa-ejecutor` | `qa-ejecutor` | parcial | Integrado | reference `playwright-operativo-avanzado.md` y stack Playwright activo |
| playwright-visual-testing | visual testing | `qa-ejecutor` | `playwright-visual-testing` | parcial | Integrado | `playwright-visual-testing` |
| postman-api | tooling API | `qa-arquitecto` | `qa-arquitecto` | parcial | Integrado | reference `tooling-api-condicional.md` en `qa-arquitecto` |
| postman-newman-automation | tooling API | `qa-arquitecto` | `qa-arquitecto` | parcial | Integrado | reference `tooling-api-condicional.md` en `qa-arquitecto` |
| production-smoke-suite | smoke/regresión | `qa-arquitecto` | `deployment-smoke-testing` | parcial | Integrado | `deployment-smoke-testing` |
| real-time-collaboration-testing | colaboración en tiempo real | `qa-ejecutor` | `qa-ejecutor` | parcial | Integrado | reference `playwright-operativo-avanzado.md` en `qa-ejecutor` |
| regression-test-selection | regresión | `qa-analista` | `regression-selection` | parcial | Integrado | `regression-selection` |
| rest-assured-api | tooling API Java | `qa-arquitecto` | `qa-arquitecto` | parcial | Integrado | reference `tooling-api-condicional.md` en `qa-arquitecto` |
| screenshot-testing-ci | visual/CI | `qa-ejecutor` | `qa-ejecutor` | parcial | Integrado | reference `playwright-operativo-avanzado.md` en `qa-ejecutor` |
| screenshot-visual-diff | visual diff | `qa-ejecutor` | `playwright-visual-testing` | parcial | Integrado | `playwright-visual-testing` |
| smoke-test-suite | smoke/regresión | `qa-arquitecto` | `deployment-smoke-testing` | parcial | Integrado | `deployment-smoke-testing` |
| sql-injection-testing | seguridad | `qa-seguridad` | `qa-seguridad` | parcial | Integrado | reference `sql-injection-no-destructivo.md` en `qa-seguridad` |
| test-parameterization | diseño de pruebas | `qa-arquitecto` | `qa-arquitecto` | parcial | Integrado | reference `configuracion-y-parametrizacion.md` en `qa-arquitecto` |
| visual-regression | visual regression | `qa-ejecutor` | `playwright-visual-testing` | parcial | Integrado | `playwright-visual-testing` |
| webapp-testing | Playwright E2E | `qa-ejecutor` | `qa-ejecutor` | parcial | Integrado | reference `playwright-operativo-avanzado.md` y stack Playwright activo |
| zap-security-scanner | seguridad | `qa-seguridad` | `qa-seguridad` | parcial | Integrado | reference `zap-baseline-controlado.md` en `qa-seguridad` |

## Resumen por acción

- `Integrado`: 42 qaskills.
- `Fusionar`: 0 qaskills.
- `Nueva skill`: 0 qaskills.
- `Nuevo agente + skill`: 0 qaskills.
- `Evaluar`: 2 qaskills.

## Estado final

### Dominios activos

- `qa-arquitecto`
- `qa-ejecutor`
- `qa-analista`
- `qa-seguridad`
- `qa-performance`
- `qa-exploratorio`
- `analista-universal-logs`

### Fuera del árbol activo por decisión actual

- `agent-browser`: queda fuera hasta que el equipo adopte explícitamente ese tooling.
- `audit-website`: queda fuera hasta que exista una necesidad concreta de auditoría transversal en performance, accessibility, SEO y best practices.

### Tooling condicional resumido

- `postman-api`, `postman-newman-automation` y `rest-assured-api` no abren dominio nuevo; quedan resumidos como tooling condicional bajo `qa-arquitecto`.

### Legacy ya absorbido por referencias o skills activas

- `allure-report-generator`, `configuration-testing` y `test-parameterization` quedan absorbidos por referencias activas del árbol principal.
- los legacy Playwright remanentes quedan absorbidos por `qa-arquitecto`, `qa-ejecutor` y la base `playwright-best-practices` sin necesidad de nuevas skills.

## Notas finales

- No se migra ningún archivo legacy 1:1 al árbol activo.
- Todo contenido activo o integrado queda en español.
- `playwright-best-practices` sigue siendo la referencia principal para evitar multiplicar skills Playwright redundantes.
