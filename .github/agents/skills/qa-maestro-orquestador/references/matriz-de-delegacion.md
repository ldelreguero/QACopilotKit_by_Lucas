# Matriz de delegacion y payloads

## Decision rapida

- setup, inicializar, configurar -> `@qa-arquitecto` con stack, tipo de proyecto y restricciones.
- planificar, estrategia, convenciones -> `@qa-arquitecto` con alcance, riesgos y sistema bajo prueba.
- implementar, ejecutar, debuggear -> `@qa-ejecutor` con spec, archivo, URL o endpoint y error.
- comparar, analizar, reportar -> `@qa-analista` con baseline, artefactos y resultado esperado.
- seguridad, owasp, auth, permisos, inyeccion -> `@qa-seguridad` con superficie, roles y endpoints a revisar.
- performance, carga, latencia, throughput, k6, artillery -> `@qa-performance` con entorno, objetivos y límites de ejecución.

## Payload minimo para delegar

- objetivo claro
- contexto relevante
- restricciones tecnicas
- artefacto esperado
- criterio de cierre

## Orden de pipeline

1. `@qa-arquitecto`
2. `@qa-ejecutor`
3. `@qa-analista`

## Ruta de seguridad

Si la necesidad principal es de seguridad, desviar fuera del pipeline general hacia `@qa-seguridad` salvo que primero falte setup o contexto técnico.

Usar esta ruta en este orden cuando haya código, endpoints o flujos autenticados:

1. baseline OWASP para priorizar superficie.
2. auth bypass si existen login, roles, sesión o recursos por identificador.
3. SQL injection no destructivo si hay búsquedas, filtros o consultas dinámicas.
4. ZAP baseline solo si hay entorno aislado y alcance explícito de scanning.
5. LLM security solo si el producto usa prompts, RAG, memoria o herramientas.

## Ruta de performance

Si la necesidad principal es performance, desviar hacia `@qa-performance` y confirmar antes ambiente, límites y tipo de campaña.

Usar esta ruta en este orden cuando el objetivo sea carga o degradación:

1. baseline k6 para smoke, load o stress gradual.
2. Artillery si el flujo se beneficia de escenarios declarativos o protocolos compatibles.
3. correlación mínima con errores, timeouts y señales del sistema.

## Antipatrones

- delegar sin prerequisitos
- enviar contexto excesivo e irrelevante
- hacer analisis manual cuando ya existe agente especialista
- ejecutar tests desde el orquestador si la tarea es operativa
