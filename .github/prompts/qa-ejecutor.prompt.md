---
description: "Implementa, ejecuta o depura tests Playwright con una salida enfocada en cambio aplicado y verificación."
agent: "qa-ejecutor"
argument-hint: "Describe el test, archivo, error o flujo que quieres implementar o depurar."
---

# QA ejecutor

Usa este prompt para pedir trabajo operativo al agente [qa-ejecutor](../agents/qa-ejecutor.agent.md) sin duplicar sus reglas completas.

## Entrada recomendada

- tipo de tarea: implementar, ejecutar, debuggear o reparar selector
- archivo o suite objetivo
- URL, endpoint o criterio de aceptación
- error observado o comando fallido si existe

## Instrucciones para esta ejecución

1. Reproduce el problema o valida el contexto antes de editar.
2. Sigue el patrón existente del repo en naming, fixtures y estructura.
3. Prioriza selectores estables y evita waits arbitrarios.
4. Corrige la causa raíz del fallo, no solo el síntoma visible.
5. Reejecuta el alcance mínimo razonable y reporta el resultado.

## Salida esperada

- archivo(s) creados o modificados
- cambio técnico aplicado
- comando de validación ejecutado
- resultado observado o limitación explícita

## Referencias

- [Skill QA Ejecutor](../agents/skills/qa-ejecutor/SKILL.md)
- [Workflow de debugging](../agents/skills/qa-ejecutor/references/workflows-de-debugging.md)
- [Playwright Skill](../agents/skills/playwright-skill/SKILL.md)
