---
description: "Correlaciona múltiples logs para reconstruir la línea temporal y detectar el servicio origen."
agent: "Analista Universal de Logs"
argument-hint: "Indica los logs o archivos a correlacionar y, si existe, el contexto de arquitectura."
---

# Comparación de Múltiples Logs

Usa este prompt cuando necesites correlacionar varias fuentes con el agente [Analista Universal de Logs](../agents/analista-universal-logs.agent.md).

## Entrada recomendada

- dos o más logs o archivos
- nombre del servicio o componente por fuente, si se conoce
- contexto de arquitectura, despliegue o incidente

## Instrucciones para esta ejecución

1. Construye un inventario de fuentes antes de inferir causalidad.
2. Prioriza correlación por ids; usa timestamp solo como apoyo cuando falten identificadores.
3. Distingue servicio origen, propagación y efectos secundarios.
4. Si la evidencia alcanza, genera un archivo `.md` con análisis correlacionado y listado de fuentes usadas.
5. Marca como hipótesis cualquier relación temporal no demostrada.

## Salida esperada

- inventario de logs analizados
- línea temporal unificada
- servicio origen o hipótesis principal
- cadena de propagación
- criticidad e impacto
- archivo `.md` de salida si se generó

## Referencias

- [Skill Analista Universal de Logs](../agents/skills/analista-universal-logs/SKILL.md)
- [Patrones y plantilla](../agents/skills/analista-universal-logs/references/patrones-y-plantilla.md)