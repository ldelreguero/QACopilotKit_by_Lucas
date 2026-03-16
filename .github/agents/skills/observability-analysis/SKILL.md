---
name: observability-analysis
description: Analiza trazas, spans, métricas y correlación entre logs y requests para detectar drift operativo o fallos instrumentados. Usar cuando el usuario pida revisar OpenTelemetry, observabilidad o evidencias de ejecución distribuidas.
license: Proprietary
compatibility: Diseñada para GitHub Copilot en VS Code con acceso a artefactos de tracing, métricas, logs o resultados exportados desde OpenTelemetry u otra capa de observabilidad.
metadata:
  author: Lucas del Reguero Martinez
  version: "1.0"
---

# Observability analysis

## Cuando usar

Usa esta skill cuando la tarea implique:

- revisar trazas o spans de una ejecución fallida
- validar que un flujo emite señales observables correctas
- correlacionar errores de API con logs, métricas o tracing
- comprobar cobertura operativa de OpenTelemetry

## Resultado esperado

La salida debe dejar:

1. señal observada y señal esperada claramente separadas
2. correlación entre trace, request, log o métrica si existe
3. huecos de instrumentación identificados
4. veredicto técnico sobre degradación, drift o falla real

## Instrucciones

1. Empieza por el flujo de negocio.
   Define qué operación se intentó ejecutar antes de leer spans o métricas aisladas.
2. Verifica la cadena mínima.
   Confirma trace raíz, spans relevantes, status, duración y atributos críticos.
3. Correlaciona señales.
   Une request, trace, logs y métricas antes de concluir que el problema es de producto o solo de observabilidad.
4. Distingue ausencia de señal de fallo funcional.
   Una instrumentación incompleta puede ocultar un flujo sano o viceversa.
5. Prioriza por impacto operativo.
   Trata como alta severidad la pérdida de trazabilidad en caminos críticos, errores sin correlación y spans con estados inconsistentes.
6. Cierra con recomendación accionable.
   Propón ajustar instrumentación, enriquecer atributos, corregir propagación o revisar el servicio origen.

## Cobertura mínima sugerida

- span raíz presente para el flujo
- propagación de trace id entre servicios
- status y duración coherentes
- atributos útiles: endpoint, operación, error type, actor o recurso
- correlación con al menos una fuente adicional: log o métrica

## Recursos relacionados

- [Correlación de señales](references/correlacion-de-senales.md)
- [qa-analista](../qa-analista/SKILL.md)

## Casos borde

- Si no hay trazas pero sí logs, declara limitación de observabilidad antes del análisis funcional.
- Si existen múltiples traces parciales, agrupa por trace id, request id o ventana temporal.
- Si la suite es local y no hay backend instrumentado, limita el análisis a señales disponibles y huecos detectados.