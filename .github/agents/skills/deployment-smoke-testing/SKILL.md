---
name: deployment-smoke-testing
description: Diseña suites smoke rápidas y seguras para post-deploy o validación continua, con foco en caminos críticos y rollback signal. Usar cuando el usuario pida smoke suites operativas para staging o producción.

compatibility: Diseñada para GitHub Copilot en VS Code con Playwright u otro runner de API/UI y con entornos donde existan cuentas o datos seguros para smoke.

---

# Deployment smoke testing

## Cuando usar

Usa esta skill cuando la tarea implique:

- definir una smoke suite de despliegue
- separar smoke de regresión completa
- validar una release en staging o producción
- decidir qué checks deben disparar rollback o stop rollout

## Resultado esperado

La salida debe dejar:

1. subset smoke priorizado por criticidad
2. restricciones de seguridad operativa explícitas
3. tiempos y umbrales razonables para ejecución continua
4. criterio de rollback o bloqueo definido

## Instrucciones

1. Optimiza para señal rápida.
   Una smoke suite no compite con regresión; debe responder rápido si la release rompió algo crítico.
2. Diseña para entorno real.
   Evita side effects permanentes, billing real, spam o mutaciones irreversibles.
3. Prioriza caminos núcleo.
   Incluye health, home, auth, flujo comercial principal y un check API crítico.
4. Haz independientes los checks.
   Cada smoke debe poder fallar sin contaminar al resto.
5. Separa staging y producción.
   En producción usa lecturas seguras o cuentas aisladas; en staging puedes permitir un poco más de profundidad.
6. Define salida operativa.
   Indica qué fallo bloquea, qué fallo alerta y cuál solo deja evidencia.

## Componentes mínimos recomendados

- health endpoint o readiness
- carga de una página o endpoint crítico
- autenticación o validación de sesión básica
- un flujo de negocio núcleo sin side effect irreversible
- verificación de assets, contratos o terceros esenciales si el sistema depende de ellos

## Recursos relacionados

- [Smoke en producción](references/smoke-en-produccion.md)
- [qa-arquitecto](../qa-arquitecto/SKILL.md)

## Casos borde

- Si el entorno productivo no tolera cuentas de prueba, limita el smoke a checks read-only.
- Si un tercero es inestable, decide explícitamente si bloquea rollout o solo alerta.
- Si el despliegue es progresivo, ajusta la smoke para dar señal antes del rollout total.