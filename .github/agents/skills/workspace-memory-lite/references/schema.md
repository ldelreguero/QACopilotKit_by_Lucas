# Schema

La memoria estructurada del workspace usa JSONL como formato canonico.

## Por que JSONL

- una entrada por linea simplifica append y diff
- se puede filtrar con busqueda textual por `key`, `tags` o `status`
- evita bloques YAML largos que generan mas conflictos de merge
- es lo bastante estructurado para agentes y lo bastante simple para revision humana

## Campos base

Cada entrada debe ser un objeto JSON valido con estos campos:

- `id`: identificador unico y estable
- `key`: clave semantica corta para deduplicacion y reemplazo
- `category`: `decision`, `constraint`, `baseline`, `preference`, `lesson`, `runbook`, `standard`
- `summary`: hecho durable en una frase
- `when_to_use`: cuando esa memoria debe influir en la tarea actual
- `tags`: lista corta de etiquetas
- `agent_scope`: agentes a los que aplica; usar `all` si corresponde a todos
- `source`: archivo, decision o chat del que proviene
- `recorded_at`: fecha ISO `YYYY-MM-DD`
- `status`: `active` o `superseded`

Campos opcionales:

- `action`: accion sugerida cuando la entrada exprese una leccion o runbook
- `supersedes`: `id` de la entrada anterior que queda reemplazada

## Ejemplo

```json
{"id":"windows-cmd-delete-2026-04-06","key":"windows.cmd.delete-dir","category":"lesson","summary":"En Windows CMD no usar Remove-Item para borrar carpetas.","when_to_use":"Cuando la shell activa sea CMD y haya que borrar directorios.","action":"Usar rmdir /s /q.","tags":["windows","cmd","delete"],"agent_scope":["all"],"source":"memory/debugging.md","recorded_at":"2026-04-06","status":"active"}
```
