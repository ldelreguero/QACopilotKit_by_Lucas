---
description: "Analiza una sola fuente de logs y produce un diagnóstico técnico con evidencia y archivo de salida."
agent: "Analista Universal de Logs"
argument-hint: "Pega el log o indica el archivo a revisar junto con contexto opcional."
---

# Análisis base de logs

Usa este prompt para pedir un análisis técnico completo sobre una sola fuente de logs con el agente [Analista Universal de Logs](../agents/analista-universal-logs.agent.md).

## Entrada recomendada

- log pegado en chat o archivo objetivo
- servicio o componente, si se conoce
- rango temporal o incidente asociado
- contexto opcional: cambios recientes, comportamiento esperado o ambiente

## Instrucciones para esta ejecución

1. Reconstruye una línea temporal mínima antes de concluir.
2. Prioriza el primer error significativo sobre los errores derivados.
3. Separa siempre hechos, inferencias e incertidumbres.
4. Si la evidencia alcanza, genera un archivo `.md` con el análisis completo y la sección Archivos Analizados.
5. Si falta contexto crítico, indícalo explícitamente junto con los datos mínimos faltantes.
6. Si confirmas una signature, causa raíz o runbook reusable, cierra con la sección exacta `Sugerencia de memoria durable (opcional)` usando la [Plantilla de sugerencia de memoria](./references/memory-suggestion-template.md).

## Salida esperada

- resumen del problema
- causa raíz más probable o hipótesis ordenadas
- evidencia principal
- impacto y criticidad
- acciones recomendadas
- archivo de salida `.md` si se generó
- sección exacta `Sugerencia de memoria durable (opcional)` si aplica

## Referencias

- [Skill Analista Universal de Logs](../agents/skills/analista-universal-logs/SKILL.md)
- [Patrones y plantilla](../agents/skills/analista-universal-logs/references/patrones-y-plantilla.md)
- [Plantilla de sugerencia de memoria](./references/memory-suggestion-template.md)