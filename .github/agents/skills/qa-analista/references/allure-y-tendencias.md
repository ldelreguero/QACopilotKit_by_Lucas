# Allure y tendencias

Esta guía resume el valor útil de la qaskill legacy de Allure para el rol analítico.

## Qué debe mirar el analista en allure

- tendencia de pasados/fallidos entre ejecuciones
- agrupación por suite, feature, historia o severidad
- diferencia entre fallo de producto, fallo de test e incidente ambiental
- adjuntos útiles: screenshots, trace, logs, payloads, videos
- contexto de entorno: browser, OS, base URL, versión o deployment target

## Lectura operativa recomendada

1. Ver cuántos fallos son nuevos respecto del baseline.
2. Separar flaky de regresión confirmada.
3. Detectar clusters por feature, endpoint o tipo de error.
4. Ver si el patrón se repite históricamente.
5. Priorizar lo que afecta caminos críticos o contratos.

## Señales de buen reporte

- tests etiquetados con severidad, feature o tags útiles
- adjuntos suficientes para diagnosticar sin reproducir localmente
- environment metadata presente
- categorías de fallo entendibles para triage
- historial preservado entre builds cuando el flujo lo permite

## Anti-patrones

- reporte bonito pero sin clasificación accionable
- no distinguir entre fallo funcional y fallo ambiental
- adjuntar artefactos masivos sin una narrativa de impacto
- perder historial entre ejecuciones y analizar cada corrida aislada

## Veredictos útiles del analista

- regresión confirmada
- riesgo alto pendiente de validación adicional
- fallo ambiental o de infraestructura
- flaky con evidencia insuficiente para tratarlo como producto

## Referencias relacionadas

- [Plantillas de análisis](plantillas-de-analisis.md)
- [QA Analista](../SKILL.md)
