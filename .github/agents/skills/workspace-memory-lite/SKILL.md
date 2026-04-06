---
name: "workspace-memory-lite"
description: "Usa memoria persistente estructurada basada en archivos JSONL para recordar contexto de proyecto, preferencias y lecciones aprendidas entre chats, sesiones y consultas. Activar cuando haya que recuperar decisiones previas, baselines, convenciones, problemas repetidos o guardar hallazgos duraderos."
---

# Workspace memory lite

Skill para memoria estructurada en workspace sin base de datos ni embeddings.

Sirve para agentes visibles e internos que necesitan continuidad entre consultas, chats o sesiones usando archivos versionables y faciles de editar.

## Archivos canonicos

- `../../../agent-memory/project-context.jsonl`
- `../../../agent-memory/user-preferences.jsonl`
- `../../../agent-memory/lessons-learned.jsonl`

## Vistas de compatibilidad humana

- `../../../agent-memory/project-context.md`
- `../../../agent-memory/user-preferences.md`
- `../../../agent-memory/lessons-learned.md`

## Protocolo recomendado

1. Determinar si la tarea realmente necesita memoria previa.
2. Elegir el bucket adecuado:
   - `project-context.jsonl` para decisiones, restricciones y baselines
   - `user-preferences.jsonl` para preferencias duraderas
   - `lessons-learned.jsonl` para fixes y patrones repetibles
3. Leer primero las entradas mas recientes y usar solo las que tengan `status: active`.
4. Filtrar por `key`, `category`, `tags` y `agent_scope` antes de mezclar memoria en la respuesta.
5. Si no hay coincidencias utiles, continuar sin forzar memoria.

## Referencias de trabajo

- `./references/schema.md`
- `./references/operations.md`
- `./references/key-examples.md`

## Regla de escritura

Antes de escribir:

- buscar entradas activas con la misma `key` o tags equivalentes
- si la memoria cambia una regla vigente, marcar la anterior como `superseded`
- escribir una sola linea JSON valida por entrada
- mantener `summary` y `when_to_use` breves y concretos

## Plantilla de sugerencia para prompts

Cuando un prompt o agente no vaya a persistir directamente pero si deba proponer una memoria, usar esta seccion exacta:

```md
## Sugerencia de memoria durable (opcional)
- bucket: `project-context.jsonl | user-preferences.jsonl | lessons-learned.jsonl`
- key: `dominio.subdominio.hecho`
- category: `decision | constraint | baseline | preference | lesson | runbook | standard`
- summary: `Hecho durable en una sola frase.`
- when_to_use: `Cuando esta memoria debe influir en una tarea futura.`
```

Si no existe hallazgo durable, omitir la seccion.

## Regla de seguridad

- nunca guardar secretos, tokens o credenciales
- nunca guardar datos personales o sensibles
- no persistir logs efimeros, stack traces completos ni salidas transitorias de CI

## Uso por rol

- Orquestador: routing, restricciones, criterios de delegacion y convenciones compartidas.
- Arquitecto: setup, estructura, tradeoffs y baselines tecnicos.
- Ejecutor y healer: fallos recurrentes, fixes robustos y notas de mantenimiento.
- Analista y logs: contratos, signatures repetidas y criterios de comparacion.
- Seguridad y performance: limites aprobados, perfiles y baselines reproducibles.
- Exploratorio, generator, planner, readme, promptfoo y tilt: preferencias operativas, restricciones de flujo y decisiones duraderas de su dominio.

## Ejemplos de keys por dominio

- Routing QA: `routing.entrypoint.qa`, `routing.playwright.worker-boundary`
- Setup QA: `qa.setup.api-project-base`, `qa.setup.ui-mcp-required`
- Ejecucion QA: `qa.exec.login-selector-fallback`, `qa.exec.api-auth-precondition`
- Analisis QA: `qa.analysis.orders-contract-baseline`, `qa.analysis.ignore-ephemeral-fields`
- Exploratorio QA: `qa.exploration.checkout-risk-invariant`, `qa.exploration.auth-ambiguity-session-timeout`
- Logs: `logs.root-cause.db-connection-pool`, `logs.runbook.kafka-rebalance-delay`
- Promptfoo: `promptfoo.provider.default-regression`, `promptfoo.baseline.cost-threshold-basic`
- Tilt: `tilt.resource.api-port`, `tilt.runbook.pending-resource-upstream-block`
- Seguridad: `security.constraint.non-destructive-sqli`, `security.baseline.authz-rbac-check`
- Performance: `performance.baseline.api-p95-staging`, `performance.constraint.max-safe-load-staging`

## Regla de mantenimiento

- evitar duplicados
- condensar entradas muy parecidas en una sola
- corregir o borrar memoria obsoleta cuando ya no aplique
- priorizar claridad antes que volumen