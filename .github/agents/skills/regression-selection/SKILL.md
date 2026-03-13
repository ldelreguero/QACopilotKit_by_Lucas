---
name: regression-selection
description: Prioriza qué tests de regresión ejecutar según cambios, riesgo, cobertura y criticidad funcional. Usar cuando el usuario pida reducir suites, seleccionar smoke/regresión o justificar cobertura incremental.
license: Proprietary
compatibility: Diseñada para GitHub Copilot en VS Code con acceso a cambios de código, historial de fallos, suites existentes o conocimiento básico del impacto funcional.
metadata:
  author: Lucas del Reguero Martinez
  version: "1.0"
---

# Regression Selection

## Cuando usar

Usa esta skill cuando la tarea implique:

- decidir qué subset de regresión correr
- priorizar suites por impacto de cambios
- recortar una suite demasiado lenta
- justificar smoke, regresión focalizada o regresión completa

## Resultado esperado

La salida debe dejar:

1. subset recomendado con criterio explícito
2. riesgos cubiertos y riesgos diferidos
3. relación entre cambios y suites seleccionadas
4. recomendación de ejecución: smoke, focalizada o completa

## Instrucciones

1. Parte del cambio real.
   Usa archivos modificados, componentes afectados, endpoints o contratos tocados como base de selección.
2. Cruza con criticidad.
   Prioriza flujos de negocio, integraciones compartidas y áreas con historial de regresión.
3. Diferencia cobertura de confianza.
   Un test existente no siempre da confianza si es flaky, superficial o ya no representa el flujo.
4. Define un corte operativo.
   Propón al menos tres capas: smoke mínimo, regresión focalizada y regresión completa.
5. Expón riesgos residuales.
   Si decides no correr una suite, explica qué podría escapar.
6. Recalibra con evidencia.
   Si el cambio toca contratos, auth, navegación central o datos compartidos, eleva el nivel recomendado.

## Señales fuertes para incluir una suite

- archivo o módulo modificado directamente relacionado
- endpoint compartido por varios flujos
- cambios en auth, permisos, checkout, pricing o búsqueda
- historial reciente de flakes o regresiones en esa zona
- contratos o schemas alterados

## Recursos relacionados

- [Impacto y recorte](references/impacto-y-recorte.md)
- [qa-analista](../qa-analista/SKILL.md)

## Casos borde

- Si no existe trazabilidad entre código y tests, usa criticidad funcional y fallos históricos como sustituto.
- Si el cambio es transversal y de bajo nivel, recomienda ampliar a full regression o al menos a suites núcleo.
- Si el tiempo de ventana es fijo, devuelve explícitamente qué queda fuera.