# Operations

## Logica de lectura

1. Decide si la tarea depende de antecedentes o si basta con el contexto actual.
2. Selecciona solo el bucket necesario:
   - `project-context.jsonl` para decisiones, restricciones y baselines
   - `user-preferences.jsonl` para preferencias duraderas
   - `lessons-learned.jsonl` para fixes y patrones repetibles
3. Prioriza entradas recientes y con `status: active`.
4. Filtra por `agent_scope`, `key`, `category` y `tags` antes de usar una entrada.
5. Si dos entradas chocan y una marca `supersedes`, usa la mas nueva y descarta la reemplazada.

## Logica de escritura

1. Persistir solo informacion durable y reusable.
2. Reutilizar una `key` consistente si el hallazgo pertenece a una familia ya existente.
3. Antes de escribir, buscar si ya existe una entrada activa equivalente.
4. Si la nueva informacion invalida una memoria vieja, actualizar la anterior a `superseded` y anotar `supersedes` en la nueva.
5. Escribir una unica linea JSON valida; no usar Markdown libre dentro del `.jsonl`.

## Regla de volumen

- no cargar memoria completa si solo hace falta una o dos entradas
- no escribir hallazgos de una sola vez que no vayan a reutilizarse
- no convertir cada ejecucion en memoria permanente

## Regla de higiene

- nunca guardar secretos, tokens, credenciales ni datos sensibles
- nunca guardar logs crudos extensos o resultados transitorios de CI
- condensar duplicados con la misma `key`
- si una entrada deja de aplicar, marcarla como `superseded`
