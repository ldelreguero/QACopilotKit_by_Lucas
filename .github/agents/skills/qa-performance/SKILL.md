---
name: qa-performance
description: Orquesta y ejecuta análisis QA de performance y carga, priorizando baseline reproducible, thresholds claros y perfiles de tráfico realistas antes de escalar campañas pesadas. Usar cuando el usuario pida diseñar, revisar o preparar pruebas de carga y performance para web o API.
license: Proprietary
compatibility: Diseñada para GitHub Copilot en VS Code con acceso a endpoints, suites de carga, documentación del sistema o señales básicas de observabilidad.
metadata:
  author: Lucas del Reguero Martinez
  version: "1.0"
---

# QA Performance

## Cuando usar

Usa esta skill cuando la tarea implique:

- definir un baseline de carga o performance
- revisar latencia, throughput, error rate o estabilidad bajo carga
- preparar scripts k6 o Artillery
- priorizar thresholds antes de una campaña más pesada

## Resultado esperado

La salida debe dejar:

1. perfil de carga recomendado
2. thresholds y métricas objetivo
3. separación entre baseline, estrés y validación exploratoria
4. alcance explícito de lo que no se ejecuta todavía

## Instrucciones

1. Empieza por el perfil real.
   Prioriza flujos, endpoints y niveles de concurrencia que representen tráfico plausible.
2. Define criterios de salida antes de correr carga.
   Toda prueba debe declarar percentiles, error rate y throughput esperados.
3. Escala de menos a más.
   Baseline, load y stress no son intercambiables; avanza por etapas.
4. Exige correlación mínima.
   Una degradación sin contexto de endpoint, fase o threshold no alcanza como hallazgo útil.
5. Separa herramienta de objetivo.
   k6 y Artillery son medios distintos; el caso de uso define cuál conviene.
6. Cierra con siguiente paso controlado.
   Toda observación debe sugerir ajuste de script, threshold o campaña posterior.

## Primera ola consolidada

Esta skill absorbe tres líneas base operativas del cluster:

- `k6-performance` como baseline principal de carga reproducible
- `artillery-load` para escenarios declarativos HTTP o WebSocket
- `artillery-load-testing` como complemento operativo del mismo stack Artillery

## Flujo recomendado

1. Delimita alcance y entorno.
   Confirma ambiente apto, ventanas de ejecución, límites de concurrencia y endpoints críticos.
2. Arranca por baseline k6.
   Usa k6 para smoke, load o stress gradual cuando el objetivo sea control fino de thresholds y percentiles.
3. Activa Artillery si el perfil lo pide.
   Usa Artillery cuando convenga modelar flujos declarativos, eventos o protocolos compatibles.
4. Correlaciona con señales mínimas.
   Relaciona resultados de carga con errores, timeouts, saturación o degradación observable.
5. Resume por impacto y siguiente prueba.
   Cierra con umbrales incumplidos, hipótesis y paso siguiente seguro.

## Recursos relacionados

- [Alcance inicial del cluster](references/alcance-inicial-del-cluster.md)
- [Baseline k6 reproducible](references/baseline-k6-reproducible.md)
- [Artillery para carga declarativa](references/artillery-para-carga-declarativa.md)

## Casos borde

- Si el entorno no es apto para carga, limita la salida a diseño, sizing y criterios de ejecución.
- Si faltan métricas mínimas del sistema, reporta hallazgos con cautela y pide correlación adicional.
- Si el usuario mezcla performance con seguridad o resiliencia, separa objetivos antes de automatizar.