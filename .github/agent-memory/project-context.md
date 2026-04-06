# Contexto persistente del proyecto

Archivo de compatibilidad humana.

La fuente canonica para agentes esta en `project-context.jsonl`.

## Resumen activo

- [2026-04-06] routing: el punto de entrada recomendado para usuarios QA es `@qa-maestro-orquestador`. Uso: cuando el pedido mezcle setup, ejecucion y analisis.
- [2026-04-06] standard: `playwright-skill` es la base canonica para reglas transversales de Playwright. Uso: cuando haya que decidir locators, waits, assertions u organizacion de tests.
- [2026-04-06] workers: `playwright-test-planner`, `playwright-test-generator` y `playwright-test-healer` se tratan como workers internos y no como entrada principal de usuario. Uso: cuando se disene routing o documentacion del kit.