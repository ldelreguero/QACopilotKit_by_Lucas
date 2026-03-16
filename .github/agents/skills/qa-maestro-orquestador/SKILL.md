---
name: qa-maestro-orquestador
description: Orquesta el flujo completo de QA decidiendo qué agente activar, en qué orden y con qué contexto. Usar cuando el usuario pida setup, ejecución, debugging, análisis o un pipeline QA end-to-end que requiera delegación entre roles especializados.

compatibility: Diseñada para GitHub Copilot en VS Code con subagentes QA disponibles en el workspace.

---

# QA maestro orquestador

## Cuándo usar

Usa esta skill cuando la solicitud requiera:

- decidir entre setup, planificación, ejecución o análisis
- distinguir entre estrategia general y workflows Playwright especializados
- coordinar varios agentes QA en secuencia
- correr un pipeline QA completo
- enrutar la tarea al agente especializado correcto con el contexto mínimo necesario

## Resultado esperado

La salida debe dejar claro:

1. qué agente se activa
2. por qué ese agente es el adecuado
3. qué contexto debe recibir
4. si la tarea es secuencial o puede dividirse

## Regla principal

El orquestador decide y delega. No debe absorber trabajo especializado si ya existe un agente responsable.

## Instrucciones

1. Clasifica la intención del usuario.
   Determina si la necesidad es setup, diseño, implementación, ejecución, debugging, comparación o análisis.
2. Evalúa prerequisitos.
   Antes de delegar, confirma si el proyecto existe, tiene dependencias, config y tests mínimos.
3. Selecciona un agente destino.
   Usa la tabla de decisión y envía solo el contexto relevante.
4. Divide en fases si hace falta.
   Para pipelines completos, separa setup, ejecución y análisis.
5. No dupliques responsabilidad.
   Si una tarea requiere crear configuración, delega al arquitecto; si requiere escribir o ejecutar tests de forma general, delega al ejecutor; si requiere comparar datos o resultados, delega al analista.
6. Usa workers internos cuando el patrón ya esté especializado.
   Si la tarea requiere planning exploratorio de UI, generación desde plan o healing iterativo, deriva al worker Playwright correspondiente en vez de ampliar artificialmente el alcance del arquitecto o del ejecutor.

## Tabla de decisión

| Señal en la solicitud | Agente destino | Contexto mínimo |
|---|---|---|
| configurar, setup, inicializar, crear proyecto | `@qa-arquitecto` | tipo de proyecto, stack y restricciones |
| planificar, estrategia, diseñar cobertura | `@qa-arquitecto` | alcance, riesgos y sistema bajo prueba |
| explorar UI, recorrer flujo, armar plan desde navegador | `@playwright-test-planner` | URL, flujo objetivo, restricciones y estado inicial |
| implementar test, escribir test, codear | `@qa-ejecutor` | spec, URL o endpoint, criterio de aceptación |
| generar test desde plan, materializar escenario | `@playwright-test-generator` | plan, escenario objetivo, seed y path esperado |
| ejecutar, correr tests, run | `@qa-ejecutor` | suite, archivo, proyecto o filtro |
| debuggear, arreglar test, falla | `@qa-ejecutor` | error, archivo, contexto de reproducción |
| recuperar suite o test roto con iteración guiada | `@playwright-test-healer` | fallo reproducible, archivo o filtro y contexto del error |
| comparar, regresión, diff, resultados | `@qa-analista` | baseline, artefactos y salida esperada |
| reporte allure, métricas, integridad | `@qa-analista` | path de resultados o reporte |
| seguridad, owasp, authz, authn, headers, inyección | `@qa-seguridad` | superficie, roles, endpoints y restricciones de entorno |
| performance, carga, latencia, throughput, k6, artillery | `@qa-performance` | entorno, endpoints, límites de ejecución y objetivo de carga |
| pipeline completo, todo el flujo | secuencial | architect -> executor -> analyst |

## Pipeline recomendado

### Fase 1: setup

- delegar a `@qa-arquitecto`
- verificar estructura, config, dependencias y reporters

### Fase 2: ejecución

- delegar a `@qa-ejecutor` para implementación y ejecución generalista
- delegar a `@playwright-test-generator` si ya existe un plan claro y el objetivo es generar código
- delegar a `@playwright-test-healer` si el objetivo principal es recuperar tests fallidos mediante iteración cerrada

### Fase 3: análisis

- delegar a `@qa-analista`
- interpretar resultados, regressions y artefactos

## Uso de MCP por parte del orquestador

El orquestador solo usa MCP para inspección rápida cuando ayuda a decidir una delegación. No debe usarlo para sustituir el trabajo operativo del ejecutor.

## Reglas de no invasión

- No ejecutar tests directamente si la tarea corresponde a `@qa-ejecutor`.
- No crear configuración si la tarea corresponde a `@qa-arquitecto`.
- No analizar manualmente payloads o reportes si la tarea corresponde a `@qa-analista`.
- No usar `@qa-arquitecto` para planning exploratorio de UI si existe necesidad real de recorrer la interfaz.
- No usar `@qa-ejecutor` para absorber por defecto la generación desde plan o el healing iterativo si ya existe el worker interno adecuado.

## Checklist previo a delegar

- el proyecto ya está inicializado
- hay dependencias instaladas o al menos package.json
- existe config razonable si se va a ejecutar
- hay baseline o resultados si se va a analizar
- el contexto enviado al subagente es suficiente y no excesivo

## Recursos relacionados

- [Matriz de delegación y payloads](references/matriz-de-delegacion.md)

## Casos borde

- Si faltan prerequisitos básicos, deriva primero a `@qa-arquitecto`.
- Si la petición mezcla varias fases, divide el trabajo en secuencia explícita.
- Si el usuario pide una acción manual puntual y no un pipeline, evita sobre-orquestar.
- Si el usuario pide algo que suena a generación automática pero no existe plan, deriva primero a `@qa-ejecutor` o `@qa-arquitecto` según falte implementación o definición.