---
description: "Clasifica una necesidad QA y decide si debe delegarse al arquitecto, ejecutor o analista."
agent: "qa-maestro-orquestador"
argument-hint: "Describe la necesidad QA completa: setup, ejecución, debugging, análisis o pipeline end-to-end."
---

# QA maestro orquestador

Usa este prompt para enrutar una necesidad QA con el agente [qa-maestro-orquestador](../agents/qa-maestro-orquestador.agent.md).

## Entrada recomendada

- objetivo del usuario
- contexto del proyecto o sistema bajo prueba
- etapa actual: setup, ejecución, debugging, análisis o mezcla
- restricciones relevantes: entorno, plazos, artefactos esperados

## Instrucciones para esta ejecución

1. Clasifica la solicitud antes de delegar.
2. Selecciona un agente primario y divide en fases solo si la tarea realmente lo requiere.
3. Envía contexto suficiente, pero no excesivo.
4. Si faltan prerequisitos, deriva primero al arquitecto.
5. Responde con la ruta de delegación y el criterio de cierre de cada fase.
6. Si identificas una decisión o restricción durable, cierra con la sección exacta `Sugerencia de memoria durable (opcional)` usando la [Plantilla de sugerencia de memoria](./references/memory-suggestion-template.md).

## Salida esperada

- clasificación de la solicitud
- agente o secuencia de agentes elegida
- contexto mínimo que recibirá cada agente
- criterio de cierre por fase si hay pipeline
- sección exacta `Sugerencia de memoria durable (opcional)` si aplica

## Referencias

- [Skill QA Maestro Orquestador](../agents/skills/qa-maestro-orquestador/SKILL.md)
- [Matriz de delegación](../agents/skills/qa-maestro-orquestador/references/matriz-de-delegacion.md)
- [Plantilla de sugerencia de memoria](./references/memory-suggestion-template.md)
