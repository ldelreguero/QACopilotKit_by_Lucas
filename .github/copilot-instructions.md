# Instrucciones del repositorio

Este repositorio define una base de customización para GitHub Copilot y VS Code usando agentes, prompts, skills e integración opcional con MCP.

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

## Convenciones de agentes

- `description` debe indicar qué hace el agente y cuándo usarlo.
- Si un agente declara `agents:`, también debe incluir la tool `agent`.
- Usa `user-invocable: false` para workers internos o subagentes que no deben aparecer en el selector principal.
- Declara `model` solo si el identificador es soportado por el editor actual.
- Usa el mínimo set de tools necesario para el rol.

## Convenciones de skills

- Cada skill debe tener una carpeta propia y un archivo `SKILL.md`.
- `name` debe coincidir con el nombre del directorio y usar lowercase-hyphen.
- `description` debe explicar con claridad cuándo activar la skill.
- Las referencias internas deben ser relativas a la raíz de la skill.

## Convenciones de prompts

- Cada prompt debe encapsular una tarea repetible y concreta.
- Si el prompt depende de una persona especializada, referencia el agente adecuado en el frontmatter.
- Evita duplicar instrucciones que ya están en el agente o en las instrucciones globales.