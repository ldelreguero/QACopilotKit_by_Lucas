---
description: "Pide un setup o diseño de framework QA con salida concreta y validable."
agent: "qa-arquitecto"
argument-hint: "Describe el tipo de proyecto, stack, alcance Web/API/Hybrid y restricciones relevantes."
---

# QA arquitecto

Usa este prompt para pedir a [qa-arquitecto](../agents/qa-arquitecto.agent.md) una propuesta o implementación de setup sin duplicar la lógica del agente.

## Entrada recomendada

- tipo de proyecto: web, api o hybrid
- stack y lenguaje del repo
- restricciones: CI, reporters, MCP, entornos, secretos
- artefactos esperados: estructura, config, fixtures, documentación o health check

## Instrucciones para esta ejecución

1. Identifica primero si el repo es nuevo, existente o requiere ajuste incremental.
2. Propón la estructura mínima viable antes de escalar complejidad.
3. Justifica dependencias y configuración; no agregues paquetes por inercia.
4. Si el repo ya tiene convenciones, extiéndelas en vez de reemplazarlas.
5. Cierra con checklist de validación inicial.

## Salida esperada

- tipo de proyecto recomendado
- artefactos a crear o modificar
- decisiones técnicas y tradeoffs
- validación mínima a ejecutar
- riesgos o decisiones pendientes

## Referencias

- [Skill QA Arquitecto](../agents/skills/qa-arquitecto/SKILL.md)
- [Blueprints de setup](../agents/skills/qa-arquitecto/references/blueprints-de-setup.md)
