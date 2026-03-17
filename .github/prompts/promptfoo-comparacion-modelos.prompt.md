---
description: "Crea una evaluacion comparativa entre modelos en promptfoo con metricas de calidad, costo y latencia."
agent: "promptfoo-evals"
argument-hint: "Indica task, dataset, modelos a comparar, umbrales de calidad y limites de costo/latencia."
---

# Promptfoo - comparacion de modelos

Usa este prompt para pedir a [promptfoo-evals](../agents/promptfoo-evals.agent.md) un benchmark comparativo reproducible entre proveedores/modelos.

## Entrada recomendada

- tarea: resumen, QA, extraccion, clasificacion, RAG, etc.
- dataset o casos representativos
- modelos/proveedores a comparar
- restricciones: presupuesto, tiempo de respuesta, formato
- criterio final de decision

## Instrucciones para esta ejecucion

1. Configurar 2 providers como minimo para comparacion side-by-side.
2. Definir `defaultTest` con controles compartidos de costo/latencia/formato.
3. Incluir asserts deterministicas por defecto y rubrics solo para calidad semantica necesaria.
4. Si hay model-graded, fijar grader provider para estabilidad de scoring.
5. Entregar salida apta para CI usando `-o output.json --no-cache`.

## Salida esperada

- configuracion de benchmark y tests
- matriz de metricas sugeridas (calidad, costo, latencia)
- comandos de ejecucion y lectura de resultados
- recomendacion basada en resultados observables

## Referencias

- [Skill promptfoo-evals](../agents/skills/prompt-foo/SKILL.md)
- [Cheatsheet promptfoo](../agents/skills/prompt-foo/references/cheatsheet.md)
