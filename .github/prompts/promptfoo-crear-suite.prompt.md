---
description: "Crea o actualiza una suite promptfoo desde cero con estructura de archivos, tests y validacion CLI."
agent: "promptfoo-evals"
argument-hint: "Indica que eval vas a medir, entradas/salidas esperadas, proveedores, umbrales y modo de ejecucion local o CI."
---

# Promptfoo - crear suite

Usa este prompt para pedir a [promptfoo-evals](../agents/promptfoo-evals.agent.md) la creacion o actualizacion de una suite de evaluacion completa.

## Entrada recomendada

- objetivo a evaluar: prompt, agente, endpoint o RAG
- formato de salida esperado: texto, JSON o tool call
- providers objetivo
- criterios de aceptacion y fallos conocidos
- limites de costo y latencia

## Instrucciones para esta ejecucion

1. Buscar primero configuraciones promptfoo existentes y reutilizar convenciones del repo.
2. Si no hay suite, crear estructura minima con `promptfooconfig.yaml`, `prompts/` y `tests/`.
3. Priorizar aserciones deterministicas y escalar a model-graded solo si aplica.
4. Usar tests file-based (`file://tests/*.yaml`) y evitar configs infladas.
5. Validar config y dejar comandos listos para ejecucion con `--no-cache`.
6. Si defines decisiones duraderas de suite, provider o criterio de corte, cierra con la sección exacta `Sugerencia de memoria durable (opcional)` usando la [Plantilla de sugerencia de memoria](./references/memory-suggestion-template.md).

## Salida esperada

- suite creada o actualizada
- rationale breve de providers y assertions
- comandos de validacion y eval
- env vars requeridas
- sección exacta `Sugerencia de memoria durable (opcional)` si aplica

## Referencias

- [Skill promptfoo-evals](../agents/skills/prompt-foo/SKILL.md)
- [Cheatsheet promptfoo](../agents/skills/prompt-foo/references/cheatsheet.md)
- [Plantilla de sugerencia de memoria](./references/memory-suggestion-template.md)
