---
name: playwright-api
description: Implementa y depura pruebas API con APIRequestContext de Playwright para REST o GraphQL, con foco en aislamiento, validacion y reutilizacion. Usar cuando el usuario pida tests API dentro del stack Playwright.
license: Proprietary
compatibility: Diseñada para GitHub Copilot en VS Code con Playwright Test; apta para proyectos API puros o flujos hibridos UI + API.
metadata:
  author: Lucas del Reguero Martinez
  version: "1.0"
---

# Playwright API

## Cuando usar

Usa esta skill cuando la tarea implique:

- escribir tests REST o GraphQL con Playwright
- crear clientes reutilizables basados en `request`
- validar respuestas, headers, contratos o tiempos
- encadenar preparacion API con validacion posterior en UI

## Resultado esperado

La salida debe dejar:

1. cliente o flujo API coherente con el repo
2. aserciones sobre status, body y headers
3. ciclo de datos controlado o cleanup explicito
4. comando minimo de validacion o bloqueo documentado

## Instrucciones

1. Usa `APIRequestContext` como base.
   Evita introducir otra libreria HTTP si el repo ya usa Playwright como runner principal.
2. Modela el dominio.
   Crea clientes o helpers por recurso cuando haya mas de un endpoint relacionado.
3. Valida mas que el status code.
   Comprueba content-type, headers relevantes, estructura del body y semantica del dato.
4. Aisla el ciclo de vida.
   Cada test debe preparar sus datos, verificarlos y limpiar lo que cree si el entorno lo permite.
5. Separa errores de producto y errores del test.
   Si falla por auth, data o contrato, reporta el origen antes de tocar la implementacion.
6. Aprovecha flujos hibridos solo cuando sumen señal.
   Usa API para preparar estado y UI para validar resultado visible, no para duplicar pasos.

## Patrones recomendados

- fixtures para auth y clientes tipados
- factories para payloads validos e invalidos
- helpers de schema o contrato cuando el proyecto los tenga
- naming por recurso y operacion: `users-api.spec.ts`, `orders-api.spec.ts`

## Recursos relacionados

- [Request context y patrones](references/request-context-y-patrones.md)
- [Playwright quality gate](../playwright-best-practices/references/quality-gate.md)
- [API test organization](../playwright-best-practices/references/test-organization.md)
- [Assertions and waiting](../playwright-best-practices/references/assertions-waiting.md)
- [qa-ejecutor](../qa-ejecutor/SKILL.md)
- [playwright-best-practices](../playwright-best-practices/SKILL.md)

## Casos borde

- Si el stack principal no es Playwright para API, no fuerces migracion; adapta el alcance.
- Si un endpoint es no deterministico, controla datos o desacopla el assertion set.
- Si la API depende de terceros, explicita mock, stub o limitacion del entorno.