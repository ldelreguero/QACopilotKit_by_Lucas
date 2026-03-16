# Instrucciones del repositorio

Este repositorio define una base de customización para GitHub Copilot y VS Code usando agentes, prompts, skills e integración opcional con MCP.

## Jerarquía normativa

Al trabajar en este repo, aplica esta jerarquía:

1. `.github/copilot-instructions.md` define la doctrina global del repositorio.
2. `.github/instructions/*.instructions.md` define reglas especializadas por tipo de archivo.
3. `PLAN-COPILOT-AGENTS.md` actúa como guía canónica para crear y mantener agents, skills y prompts.
4. `README.md` está orientado al usuario final que usa el sistema, no a quien lo diseña.

Si hay tensión entre documentos, prioriza mantener la separación de audiencias y de capas.

## Estructura objetivo

- Los agentes viven en `.github/agents/*.agent.md`.
- Las skills activas viven en `.github/agents/skills/<skill-name>/SKILL.md`.
- Los prompts reutilizables viven en `.github/prompts/*.prompt.md`.
- Las instrucciones especializadas viven en `.github/instructions/*.instructions.md`.
- La configuración MCP del workspace vive en `.vscode/mcp.json`.

## Reglas de edición

- Mantén las definiciones pequeñas y enfocadas por responsabilidad.
- No mezcles reglas globales del proyecto dentro de un agente; usa instrucciones para eso.
- No conviertas una skill en un prompt gigante: usa `SKILL.md` para el flujo principal y mueve detalle a `references/` o `scripts/`.
- Conserva YAML frontmatter válido en `.agent.md`, `.prompt.md`, `.instructions.md` y `SKILL.md`.
- En documentación y prompts, escribe en español cuando el contenido sea operativo para el equipo.
- En snippets de código, nombres de propiedades y comandos, usa el idioma natural del tooling, normalmente inglés.
- Si un documento crece y cambia de audiencia, sepáralo en vez de mezclar usos distintos en el mismo archivo.

## Doctrina de arquitectura

- Usa agents para roles especializados con tools, límites y posible delegación.
- Usa skills para conocimiento reusable, no para tareas puntuales.
- Usa prompts para tareas concretas y repetibles.
- Usa instructions para reglas transversales del repo o por tipo de archivo.
- Mantén visible al usuario solo el catálogo que necesita; los workflows internos deben vivir como workers.
- Si un workflow es interno, usa `user-invocable: false`.
- Si una regla o checklist va a ser compartida por varios agentes, no la dupliques; muévela a una skill o a `references/`.

## Doctrina específica de documentación

- `README.md` debe explicar cómo un usuario final usa el sistema.
- `PLAN-COPILOT-AGENTS.md` debe explicar cómo un autor crea y mantiene el sistema.
- No conviertas el README de usuario en documento de arquitectura interna.
- No conviertas el plan canónico en una guía de uso final.

## Doctrina específica de Playwright

- `playwright-skill` es la fuente canónica para estándares transversales Playwright.
- Esa base proviene de `https://github.com/testdino-hq/playwright-skill` y debe instalarse con `npx skills add testdino-hq/playwright-skill`.
- Agents y skills QA deben referenciar esa base para locators, waits, assertions, test organization, debugging y reglas generales de calidad.
- No mantener checklists Playwright paralelos dentro de varios agents.
- Si un workflow Playwright es especializado y no debe ser visible al usuario, modelarlo como worker interno.

## Convenciones de agentes

- `description` debe indicar qué hace el agente y cuándo usarlo.
- Si un agente declara `agents:`, también debe incluir la tool `agent`.
- Usa `user-invocable: false` para workers internos o subagentes que no deben aparecer en el selector principal.
- Declara `model` solo si el identificador es soportado por el editor actual.
- Usa el mínimo set de tools necesario para el rol.
- Evita agents monolíticos que mezclen setup, ejecución, análisis y reporting sin una razón fuerte.
- Si el agent empieza a acumular conocimiento reusable, extrae ese contenido a una skill o a `references/`.
- Si el agent existe solo para una fase interna de un pipeline, trátalo como worker interno.

## Convenciones de skills

- Cada skill debe tener una carpeta propia y un archivo `SKILL.md`.
- `name` debe coincidir con el nombre del directorio y usar lowercase-hyphen.
- `description` debe explicar con claridad cuándo activar la skill.
- Las referencias internas deben ser relativas a la raíz de la skill.
- Mantén `SKILL.md` corto, enfocado y reusable; mueve detalle extenso a `references/`.
- No crees una skill nueva si el contenido pertenece claramente a una skill existente o a la base canónica del dominio.

## Convenciones de prompts

- Cada prompt debe encapsular una tarea repetible y concreta.
- Si el prompt depende de una persona especializada, referencia el agente adecuado en el frontmatter.
- Evita duplicar instrucciones que ya están en el agente o en las instrucciones globales.
- Si un prompt empieza a parecer un workflow completo con decisiones propias, probablemente necesita un agent y no un prompt.