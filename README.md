# templateAgentes

Base de trabajo para definir customizaciones de GitHub Copilot y VS Code con agentes, prompts, skills e integración opcional con MCP.

## Estructura principal

- `.github/copilot-instructions.md`: reglas globales del repositorio
- `.github/instructions/`: instrucciones por tipo de archivo o dominio
- `.github/prompts/`: prompts reutilizables
- `.github/agents/`: agentes personalizados
- `.github/agents/skills/`: skills activas del workspace
- `.vscode/mcp.json`: configuración MCP del workspace

## Uso rápido

1. Define reglas globales en `.github/copilot-instructions.md`.
2. Añade agentes en `.github/agents/` según el rol.
3. Añade skills en `.github/agents/skills/` cuando necesites capacidades reutilizables.
4. Usa `.github/prompts/` para tareas repetitivas.
5. Configura `.vscode/mcp.json` solo si necesitas tools externas.

## Estado actual

- Agentes QA y de análisis de logs disponibles en `.github/agents/`.
- Skills activas disponibles en `.github/agents/skills/`.
- MCP configurado para Playwright en `.vscode/mcp.json`.
- La migración desde el árbol legacy `qaskills` quedó estabilizada en `QASKILLS-MATRIZ.md` con 42 qaskills integradas, 0 en fusión pendiente y 2 mantenidas fuera del árbol activo por decisión explícita.
- `playwright-best-practices` actúa como fuente canónica para estándares Playwright transversales del repo.

## Matriz repo vs plan

| Área | Esperado por el plan | Estado actual del repo | Nivel |
| ---- | -------------------- | ---------------------- | ----- |
| Instrucciones globales | `.github/copilot-instructions.md` presente y operativo | Implementado | Escalable |
| Instrucciones por dominio | `.github/instructions/*.instructions.md` por tipo de archivo | Implementado para agentes, prompts y skills | Escalable |
| Agentes personalizados | `.github/agents/*.agent.md` con frontmatter válido y tools acotadas | Implementado y normalizado semánticamente | Escalable |
| Prompts reutilizables | `.github/prompts/*.prompt.md` con frontmatter y foco en una tarea concreta | Implementado; prompts compactados y referenciando agentes/skills | Escalable |
| Skills activas | `.github/agents/skills/<skill-name>/SKILL.md` | Implementado | Escalable |
| Referencias de skills | `references/` cuando la skill necesita detalle adicional | Implementado en skills custom y en `playwright-best-practices` | Escalable |
| Scripts o assets en skills | `scripts/` o `assets/` solo si agregan valor real | No adoptado aún en skills custom; hoy no hay necesidad validada | MVP |
| Documentación raíz | `README.md` con estructura y uso del repo | Implementado | MVP |
| Integración MCP | `.vscode/mcp.json` opcional según necesidad | Implementado solo para Playwright | MVP |
| Higiene semántica | nombres de tools y referencias internas alineadas al estándar actual | Implementado en agentes, prompts y skills activas | Escalable |

## Observaciones

- El repo ya alcanzó nivel escalable en la capa de instrucciones, agentes, prompts, skills y referencias.
- Los elementos que siguen en nivel MVP son opcionales por diseño: scripts/assets dentro de skills custom y una integración MCP acotada a Playwright.
- El árbol legacy `qaskills` ya no forma parte de la capa activa; la migración quedó resumida en `QASKILLS-MATRIZ.md`.
- Los agentes y skills QA deben referenciar `playwright-best-practices` para locators, waits, POM, assertions, estructura y debugging en vez de duplicar checklists locales.

## Cierre de migración qaskills

- Dominios activos consolidados: `qa-arquitecto`, `qa-ejecutor`, `qa-analista`, `qa-seguridad`, `qa-performance`, `qa-exploratorio` y `analista-universal-logs`.
- Tooling condicional absorbido bajo `qa-arquitecto`: Postman, Newman y REST Assured.
- Legacy mantenido fuera del árbol activo: `agent-browser` y `audit-website`.
