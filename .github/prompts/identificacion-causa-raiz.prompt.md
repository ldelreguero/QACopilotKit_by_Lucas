---
description: "Fuerza un análisis centrado en causa raíz, cadena causal e hipótesis descartadas."
agent: "Analista Universal de Logs"
argument-hint: "Pega el log o la evidencia y aclara si quieres foco exclusivo en causa raíz."
---

# Identificación de causa raíz

Usa este prompt cuando el objetivo principal sea aislar la causa raíz, no solo describir síntomas.

## Entrada recomendada

- log o conjunto de evidencias
- contexto del incidente
- cambio reciente o sospecha inicial, si existe

## Instrucciones para esta ejecución

1. Distingue síntoma visible, errores derivados y causa candidata.
2. Profundiza hasta donde la evidencia permita; no fuerces una causa única si no está soportada.
3. Expón cadena causal y criterios de validación de la hipótesis principal.
4. Documenta hipótesis descartadas y qué evidencia faltaría para elevar certeza.
5. Si corresponde, genera un `.md` con el análisis completo.
6. Si confirmas una causa raiz reusable, una restriccion del entorno o un runbook estable, cierra con la sección exacta `Sugerencia de memoria durable (opcional)` usando la [Plantilla de sugerencia de memoria](./references/memory-suggestion-template.md).

## Salida esperada

- síntoma visible
- causa raíz más probable o conjunto ordenado de hipótesis
- cadena causal reconstruida
- hipótesis descartadas
- acciones correctivas y validaciones pendientes
- sección exacta `Sugerencia de memoria durable (opcional)` si aplica

## Referencias

- [Skill Analista Universal de Logs](../agents/skills/analista-universal-logs/SKILL.md)
- [Patrones y plantilla](../agents/skills/analista-universal-logs/references/patrones-y-plantilla.md)
- [Plantilla de sugerencia de memoria](./references/memory-suggestion-template.md)