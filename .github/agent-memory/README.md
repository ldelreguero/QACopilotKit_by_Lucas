# Memoria estructurada de agentes

Esta carpeta guarda memoria persistente estructurada para los agentes del workspace.

## Objetivo

- recordar contexto estable entre chats y sesiones
- evitar que cada agente reinterprete la misma decision desde cero
- conservar runbooks breves, restricciones y lecciones reutilizables
- mantener una fuente versionable, revisable y facil de depurar

## Formato canonico

La fuente canonica es JSONL, no Markdown libre.

Se eligio JSONL porque:

- permite append por entrada sin reescribir archivos completos
- reduce conflictos de merge frente a YAML con listas largas
- facilita busqueda por `key`, `category`, `tags`, `agent_scope` y `status`
- mantiene estructura suficiente sin depender de una base externa

Markdown queda solo como resumen humano y compatibilidad.

## Archivos canonicos

- `project-context.jsonl`: decisiones, restricciones, convenciones y baselines del workspace
- `user-preferences.jsonl`: preferencias duraderas del usuario o del equipo
- `lessons-learned.jsonl`: fallos recurrentes, fixes reutilizables y pequenas guias de accion

## Vistas de compatibilidad

- `project-context.md`
- `user-preferences.md`
- `lessons-learned.md`

Estos archivos no son la fuente primaria para agentes. Sirven como resumen rapido para personas.

## Esquema base por entrada

Cada linea debe ser un objeto JSON valido con esta base:

```json
{
  "id": "routing-entrypoint-2026-04-06",
  "key": "routing.entrypoint",
  "category": "decision",
  "summary": "El punto de entrada recomendado para usuarios QA es @qa-maestro-orquestador.",
  "when_to_use": "Cuando el pedido mezcle setup, ejecucion y analisis.",
  "tags": ["routing", "qa", "entrypoint"],
  "agent_scope": ["qa-maestro-orquestador", "qa-arquitecto"],
  "source": "README_QACopilotKit_by_Lucas.md",
  "recorded_at": "2026-04-06",
  "status": "active"
}
```

Campos opcionales utiles:

- `action`: accion breve sugerida cuando la entrada sea una leccion o runbook
- `supersedes`: `id` de la memoria reemplazada

## Reglas operativas

- guardar solo informacion durable y reutilizable
- no guardar secretos, tokens, credenciales ni datos sensibles
- preferir una entrada nueva con `status: active` y marcar la anterior como `superseded` cuando cambie una regla
- deduplicar por `key` antes de escribir
- leer desde las entradas mas recientes y filtrar por `status: active`
