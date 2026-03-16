# Playwright operativo avanzado

Esta guía concentra el delta útil de la primera ola de qaskills Playwright para ejecución y debugging.

## Cuándo usar CLI, MCP o test runner

- `CLI`: exploración rápida, reproducción de flujos, snapshots, tab management o diagnóstico sin escribir todavía tests.
- `MCP`: inspección visual, accessibility tree, consola, network o verificación puntual en vivo.
- `@playwright/test`: automatización estable, suites repetibles, CI y validación final.

Regla práctica: primero reproduce de la forma más barata; después automatiza.

## Console errors: criterio operativo

Trata como señal fuerte y potencialmente bloqueante:

- uncaught exceptions
- unhandled promise rejections
- TypeError, ReferenceError o SyntaxError
- errores de hidratación o runtime de frameworks

No silencies por defecto. Si necesitas ignorar mensajes benignos, usa allowlist explícita y revisable.

## Network testing: qué sí conviene hacer

- interceptar requests solo cuando aísla dependencias externas o acelera escenarios inestables
- validar response status, payload y side effects observables
- usar HAR o mocks solo para escenarios concretos, no como reemplazo permanente de integración real
- correlacionar fallo visual con request fallida antes de tocar selectors o waits

## Colaboración en tiempo real

Para flujos multiusuario:

- usar contextos o sesiones separadas por rol
- definir claramente qué usuario dispara la acción y cuál observa el efecto
- validar consistencia eventual, no solo reacción inmediata
- registrar orden de eventos si hay WebSocket, presence o edición concurrente

## Screenshot y visual en CI

Para reducir falsos positivos:

- fijar viewport, locale, timezone y reduced motion cuando aplique
- estabilizar data y estados visuales antes de capturar
- evitar snapshots de pantallas demasiado volátiles
- separar visual smoke de visual exhaustivo
- revisar cambios de baseline con criterio funcional, no solo pixel-perfect

## Señales para abrir debugging más profundo

- falla solo en CI
- falla solo en paralelo
- error aparece tras navegación o cambio grande del DOM
- selector correcto, pero estado de negocio incorrecto
- hay console errors o requests fallidas cerca del punto de fallo

## Referencias relacionadas

- [Workflow operativo y debugging](workflows-de-debugging.md)
- [Playwright Best Practices](../../playwright-best-practices/SKILL.md)
- [Console Errors](../../playwright-best-practices/references/console-errors.md)
- [Advanced Network Interception](../../playwright-best-practices/references/network-advanced.md)
- [Multi-User](../../playwright-best-practices/references/multi-user.md)
- [Debugging](../../playwright-best-practices/references/debugging.md)
- [Mobile Testing](../../playwright-best-practices/references/mobile-testing.md)
