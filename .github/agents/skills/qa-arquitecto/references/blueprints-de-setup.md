# Blueprints de setup

## Web minimo

- tests/ui/
- fixtures/
- utils/
- data/
- playwright.config.ts o .js
- .env.example

## API minimo

- tests/api/contracts/
- tests/api/integration/
- fixtures/
- data/schemas/
- playwright.config.ts o .js
- .env.example

## Hybrid minimo

- tests/ui/
- tests/api/
- tests/hybrid/
- fixtures/
- utils/
- data/

## Decision MCP

Usar MCP solo si hace falta:

- inspeccion visual del DOM
- debugging interactivo
- generacion asistida de locators

No usar MCP por defecto cuando el flujo es API puro o el setup no necesita interaccion visual.

## Validacion inicial

- package.json existe
- Playwright compila
- variables de entorno definidas
- reporter justificado
- estructura alineada con el tipo de proyecto
