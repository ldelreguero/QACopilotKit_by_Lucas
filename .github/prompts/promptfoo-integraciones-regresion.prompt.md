---
description: "Configura evals promptfoo para integraciones (LangChain, Assistants, agentes) y control de regresiones en CI."
agent: "promptfoo-evals"
argument-hint: "Describe framework o integracion, contrato de salida, riesgos de regresion y pipeline CI objetivo."
---

# Promptfoo - integraciones y regresion

Usa este prompt para pedir a [promptfoo-evals](../agents/promptfoo-evals.agent.md) una suite de regresion para integraciones de frameworks o agentes.

## Entrada recomendada

- integracion objetivo: LangChain, Assistants, LangGraph, CrewAI, API custom, etc.
- flujo critico a proteger
- regresiones historicas o fallas frecuentes
- politica de corte en CI (pass/fail)

## Instrucciones para esta ejecucion

1. Modelar casos de regresion reales en tests versionados.
2. Validar estructura de salida (por ejemplo JSON schema) y reglas de negocio clave.
3. Incluir checks de costo/latencia para evitar degradaciones silenciosas.
4. Preparar comandos para ejecucion local y CI sin cache.
5. Documentar variables de entorno e indicadores que deben vigilarse.
6. Si confirmas regresiones historicas, contratos duraderos o thresholds estables, cierra con la sección exacta `Sugerencia de memoria durable (opcional)` usando la [Plantilla de sugerencia de memoria](./references/memory-suggestion-template.md).

## Salida esperada

- suite de regresion lista para pipeline
- cobertura de casos criticos
- comandos y recomendaciones de integracion CI
- riesgos pendientes y siguientes iteraciones
- sección exacta `Sugerencia de memoria durable (opcional)` si aplica

## Referencias

- [Skill promptfoo-evals](../agents/skills/prompt-foo/SKILL.md)
- [Cheatsheet promptfoo](../agents/skills/prompt-foo/references/cheatsheet.md)
- [Plantilla de sugerencia de memoria](./references/memory-suggestion-template.md)
