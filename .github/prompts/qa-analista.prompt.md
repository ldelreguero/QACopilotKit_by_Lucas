---
description: "Ejecuta un análisis QA puntual sobre artefactos, respuestas o reportes y devuelve un veredicto accionable."
agent: "qa-analista"
argument-hint: "Indica baseline, artefactos a comparar, reporte o JSON a analizar y el resultado esperado."
---

# QA analista

Usa este prompt para pedir un análisis puntual al agente [qa-analista](../agents/qa-analista.agent.md) sin repetir su manual operativo.

## Entrada recomendada

- baseline o expected data
- artefactos actuales: JSON, reportes, resultados o payloads
- objetivo del análisis: regresión, integridad, diff, contrato o resumen
- contexto opcional sobre cambios esperados

## Instrucciones para esta ejecución

1. Clasifica primero el tipo de análisis solicitado.
2. Normaliza ruido antes de concluir: ids efímeros, timestamps variables y orden no significativo.
3. Prioriza compatibilidad, semántica e impacto funcional sobre diferencias cosméticas.
4. Si falta baseline, decláralo y usa uno provisional solo si es imprescindible.
5. Cierra con veredicto y siguiente acción sugerida.

## Salida esperada

- tipo de análisis aplicado
- hallazgos ordenados por severidad
- paths o campos afectados
- veredicto final: sin cambios relevantes, cambio esperado, riesgo o regresión confirmada
- siguiente acción sugerida

## Referencias

- [Skill QA Analista](../agents/skills/qa-analista/SKILL.md)
- [Plantillas de análisis](../agents/skills/qa-analista/references/plantillas-de-analisis.md)
