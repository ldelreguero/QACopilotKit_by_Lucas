# Plan Nuevo: Bootstrap de GitHub Copilot Custom Agents

## TL;DR

Este plan define cómo preparar un workspace para crear y operar agentes personalizados de GitHub Copilot usando archivos `.agent.md` y skills reutilizables basadas en `SKILL.md`.
El foco es: **estructura de directorios**, **capas de customización**, **configuración mínima**, **buenas prácticas oficiales**, **formato interoperable de skills**, y **checklist de validación** para que los agentes aparezcan, invoquen correctamente en VS Code y reutilicen conocimiento especializado sin romper compatibilidad.

---

## Objetivo

- Estandarizar la creación de agentes en un repositorio local.
- Estandarizar la creación de skills portables basadas en la especificación abierta de Agent Skills.
- Incorporar buenas prácticas oficiales de GitHub Copilot y VS Code para agentes, tools, contexto, planning y subagents.
- Dejar una base reutilizable para equipos (orquestador + subagentes).
- Mantener configuración simple, extensible y versionable.

---

## Fuentes base

### GitHub Copilot

- `https://docs.github.com/es/copilot/how-tos/use-copilot-agents/coding-agent/create-custom-agents`
- `https://docs.github.com/es/copilot/concepts/agents/coding-agent/about-custom-agents`
- `https://docs.github.com/es/copilot/how-tos/use-copilot-agents/coding-agent/create-custom-agents#configuring-an-agent-profile`

### VS Code Copilot

- `https://code.visualstudio.com/docs/copilot/customization/custom-agents`
- `https://code.visualstudio.com/docs/copilot/overview`
- `https://code.visualstudio.com/docs/copilot/concepts/tools`
- `https://code.visualstudio.com/docs/copilot/agents/overview`
- `https://code.visualstudio.com/docs/copilot/concepts/context`
- `https://code.visualstudio.com/docs/copilot/concepts/customization`
- `https://code.visualstudio.com/docs/copilot/agents/agents-tutorial`
- `https://code.visualstudio.com/docs/copilot/agents/planning`
- `https://code.visualstudio.com/docs/copilot/agents/agent-tools`
- `https://code.visualstudio.com/docs/copilot/agents/subagents`
- `https://code.visualstudio.com/docs/copilot/agents/local-agents`
- `https://code.visualstudio.com/docs/copilot/agents/copilot-cli`
- `https://code.visualstudio.com/docs/copilot/guides/context-engineering-guide`
- `https://code.visualstudio.com/docs/copilot/guides/customize-copilot-guide`

### Agent Skills

- `https://agentskills.io/home`
- `https://agentskills.io/specification`
- `https://agentskills.io/client-implementation/adding-skills-support`

Nota:

- Este plan usa GitHub Docs y VS Code Docs como base principal para `.agent.md`.
- Para skills, usa la documentación oficial de Agent Skills como especificación de formato y diseño.

---

## Decisiones base

| Decisión | Valor recomendado | Nota |
| --- | --- | --- |
| Ubicación principal | `.github/agents/*.agent.md` | Compartida por equipo en repo |
| Compatibilidad Claude | `.claude/agents/*.md` | Opcional para flujos híbridos |
| Ubicación de skills | `.github/agents/skills/<skill-name>/SKILL.md` | Convención adoptada por este repositorio |
| Modelo | `Opcional` | Si se declara, elegir uno soportado por el editor usando autocompletado |
| Convención de nombres | `dominio-rol.agent.md` | Ej. `qa-orquestador.agent.md` |
| Convención de nombre de skill | `lowercase-hyphen` | Debe coincidir con el nombre del directorio |
| Base documental para skills | `agentskills.io` | Respetar especificación, progressive disclosure y referencias relativas |
| Instrucciones globales | `.github/copilot-instructions.md` | Punto de partida para contexto persistente del proyecto |
| Instrucciones por dominio | `.github/instructions/*.instructions.md` | Reglas específicas por tipo de archivo o área |
| Prompts reutilizables | `.github/prompts/*.prompt.md` | Para tareas repetibles invocables por slash command |

### Decisión específica para Playwright

- `playwright-best-practices` es la fuente canónica para buenas prácticas Playwright transversales del repositorio.
- Agentes y skills QA deben referenciar esa base y sus `references/` en lugar de duplicar quality gates, estándares obligatorios o convenciones de estructura.

---

## Capas de customización

Basado en la documentación de VS Code, conviene pensar la personalización en capas y no cargar toda la lógica en un solo agente.

| Capa | Cuándo usarla | Archivo recomendado |
| --- | --- | --- |
| Instrucciones globales | Reglas que deben aplicar siempre | `.github/copilot-instructions.md` |
| Instrucciones por archivo | Convenciones para módulos o extensiones específicas | `.github/instructions/*.instructions.md` |
| Prompt file | Tareas repetibles invocadas por slash command | `.github/prompts/*.prompt.md` |
| Custom agent | Persona especializada con tools y handoffs | `.github/agents/*.agent.md` |
| Skill | Capacidades reutilizables con instrucciones, scripts y recursos | `.github/agents/skills/*/SKILL.md` |
| MCP | Acceso a sistemas externos o tooling especializado | `.vscode/mcp.json` |
| Hooks | Automatización determinística en puntos del ciclo del agente | Según configuración de hooks |

Regla práctica:

- No usar un agente para reglas globales del repo; eso va en instrucciones.
- No usar una skill para una tarea puntual de un solo uso; eso va mejor en prompt file.
- No usar MCP si el problema puede resolverse con tools built-in o contexto local.

---

## Estructura de directorios (MVP)

```text
.github/
└── agents/
    ├── maestro-orquestador.agent.md
    ├── arquitecto.agent.md
    ├── ejecutor.agent.md
    └── analista.agent.md
README.md
```

### Estructura recomendada (escalable)

```text
.github/
├── copilot-instructions.md
├── instructions/
│   ├── agents.instructions.md
│   ├── prompts.instructions.md
│   └── skills.instructions.md
├── prompts/
│   ├── plan.prompt.md
│   └── review.prompt.md
├── agents/
│   ├── agente.agent.md
│   └── skills/
│       └── mi-skill/
│           ├── SKILL.md
│           ├── references/
│           │   └── guia.md
│           ├── scripts/
│           │   └── helper.js
│           └── assets/
│               └── template.json
└── README.md
.vscode/
└── mcp.json
```

### Convención recomendada para skills

```text
.github/
└── agents/
    └── skills/
        └── skill-name/
            ├── SKILL.md          # Requerido: metadata + instrucciones
            ├── scripts/          # Opcional: código ejecutable
            ├── references/       # Opcional: documentación detallada
            ├── assets/           # Opcional: plantillas o recursos
            └── ...
```

Notas:

- En este repositorio, ubicar las skills en `.github/agents/skills/` para mantenerlas junto a la definición de agentes.
- El directorio de la skill debe coincidir exactamente con `name` en el frontmatter.
- Si una skill crece, mover detalle a `references/` o `scripts/` en vez de inflar `SKILL.md`.

## Plantilla estándar de agente (`.agent.md`)

```markdown
---
name: "Mi Agente"
description: "Descripción breve del agente"
tools:
  - search
  - edit
  - fetch
agents:
  - "*"
argument-hint: "Describe lo que necesitas..."
user-invocable: true
disable-model-invocation: false
target: vscode
handoffs:
  - label: "Siguiente paso"
    agent: implementation
    prompt: "Implementa el plan anterior."
    send: false
---

# Instrucciones del agente

Reglas operativas, alcance, entradas/salidas y criterios de aceptación.
```

### Reglas de diseño para agentes

- `description` es obligatoria y debe explicar claramente qué hace el agente y cuándo usarlo.
- Si omites `tools`, el agente puede acceder a todas las tools disponibles; esto debe evitarse salvo en agentes realmente generales.
- Si declaras `agents`, debes incluir también la tool `agent`.
- `target` es opcional, pero conviene usar `vscode` cuando el agente está diseñado para el IDE.
- `model` es opcional; si se define, usar un identificador válido soportado por el editor.
- El cuerpo Markdown debe contener instrucciones operativas, límites, criterios de calidad y cuándo delegar o no.
- El prompt del agente no debe convertirse en un manual enciclopédico; debe optimizar comportamiento, no duplicar documentación general del proyecto.
- El cuerpo del agente tiene límite práctico; GitHub documenta un máximo de 30.000 caracteres para el prompt del perfil.

### Buenas prácticas para agentes

- Diseñar agentes por responsabilidad, no por tecnología genérica.
- Aplicar principio de mínimo privilegio: planning y review con tools de lectura; implementación con edición y terminal solo cuando haga falta.
- Separar planificación, implementación, revisión y análisis cuando el flujo sea complejo.
- Usar `handoffs` para transiciones guiadas entre agentes.
- Usar `user-invocable: false` en workers internos o subagentes que no deberían aparecer en el selector principal.
- Usar `disable-model-invocation: true` cuando un agente solo deba ejecutarse explícitamente y no como subagente genérico.
- Revisar periódicamente el set de tools para evitar agentes demasiado poderosos o demasiado ambiguos.

### Ubicaciones válidas

- Workspace: `.github/agents/nombre.agent.md`
- Perfil de usuario: carpeta de perfil de VS Code
- Compatibilidad Claude: `.claude/agents/nombre.md`

---

## Plantilla estándar de skill (`SKILL.md`)

Basar la creación de skills en la especificación de Agent Skills:

- Overview: `https://agentskills.io/home`
- Specification: `https://agentskills.io/specification`
- Client implementation: `https://agentskills.io/client-implementation/adding-skills-support`

```markdown
---
name: mi-skill
description: Describe qué hace la skill y cuándo debe usarse. Debe ser específica y ayudar al agente a decidir su activación.
license: Proprietary
compatibility: Opcional. Solo si requiere entorno, herramientas o permisos particulares.
metadata:
  author: Lucas del Reguero Martinez
  version: "1.0"
allowed-tools: Opcional. Soporte depende del cliente.
---

# Mi Skill

## Cuándo usar

Usa esta skill cuando...

## Instrucciones

1. Hacer X.
2. Verificar Y.
3. Si aplica, consultar [guía adicional](references/guia.md).

## Casos borde

- Caso 1
- Caso 2
```

### Reglas de formato para skills

- `name` es obligatorio, en minúsculas con guiones, máximo 64 caracteres y debe coincidir con el directorio padre.
- `description` es obligatoria, no vacía, máximo 1024 caracteres, y debe explicar qué hace la skill y cuándo usarla.
- `compatibility` solo debe agregarse si hay requisitos reales de entorno.
- `metadata` debe mantenerse pequeña y estable.
- `allowed-tools` es experimental; usarlo solo si el cliente realmente lo soporta.

### Recomendaciones de diseño

- Mantener `SKILL.md` enfocado y preferentemente por debajo de 500 líneas.
- Aplicar progressive disclosure: catálogo breve al inicio, instrucciones completas al activar la skill, recursos bajo demanda.
- Referenciar archivos con rutas relativas desde la raíz de la skill, por ejemplo `references/guia.md` o `scripts/helper.js`.
- Evitar cadenas profundas de referencias; la skill debe ser navegable con pocos saltos.
- Poner procedimientos, decisiones y criterios de uso en `SKILL.md`; mover contenido extenso a `references/`.
- Si la skill necesita scripts, documentar dependencias y errores esperables de forma explícita.

### Cuándo usar skill vs agente

- Usar agente cuando necesitas una persona persistente con tools, modelo, handoffs o restricciones.
- Usar skill cuando necesitas una capacidad reusable, portable y cargada bajo demanda.
- Usar ambos juntos cuando un agente especializado debe apoyarse en una skill de dominio o workflow.

---

## Contexto y documentación

La documentación oficial de VS Code enfatiza que la calidad del resultado depende del contexto realmente visible por el modelo.

### Recomendaciones

- Mantener `.github/copilot-instructions.md` como capa de contexto persistente del proyecto.
- Añadir `.instructions.md` por área cuando distintas zonas del repo usen reglas distintas.
- Empezar con contexto pequeño y ampliarlo solo cuando se detecten errores repetidos.
- Mantener sesiones separadas para planificación, implementación, debugging y análisis, evitando mezclar contextos.
- Referenciar archivos concretos, documentación y URLs relevantes en lugar de depender de supuestos implícitos.
- Tratar documentos como `PRODUCT.md`, `ARCHITECTURE.md` y `CONTRIBUTING.md` como artefactos vivos que alimentan mejores decisiones del agente.

### Anti-patrones

- volcar demasiada información irrelevante al prompt
- mezclar reglas contradictorias entre instrucciones, prompts y agentes
- usar una única sesión larga para tareas no relacionadas
- asumir que el agente recuerda sesiones anteriores

---

## Tools y seguridad

La documentación oficial recomienda controlar las tools con intención y no habilitarlas indiscriminadamente.

### Principios

- Seleccionar solo las tools necesarias para el rol del agente.
- Preferir tools built-in para lectura, búsqueda y edición local antes de agregar MCP.
- Agregar MCP solo para necesidades externas reales: APIs, browsers, bases de datos, cloud, etc.
- Si un agente usa subagentes, incluir `agent` y restringir `agents:` a los nombres concretos que debe poder invocar.
- Para orquestadores, usar handoffs y subagentes; para workers, reducir superficie de tools.

### Seguridad operativa

- Evitar auto-approval general salvo en entornos controlados.
- Revisar cuidadosamente tools con efectos laterales: edición, terminal, fetch y tools externas.
- Para tareas sensibles, usar agentes read-only o sessions con aprobación manual.
- En Windows, preferir PowerShell antes que CMD para mejor integración del terminal agent.

---

## Patrones recomendados

### Plan -> Implement -> Review

Basado en la documentación de planning y context engineering:

1. Crear o refinar contexto global del proyecto.
2. Usar Plan o un agente planificador para producir plan estructurado.
3. Implementar en la misma sesión o delegar a Copilot CLI si el trabajo es autónomo y bien definido.
4. Revisar contra el plan antes de cerrar.

### Coordinator -> Workers

Basado en la documentación de subagents:

- Un coordinador decide fases y delega.
- Los workers tienen foco estrecho y tools mínimas.
- Cada subagente trabaja con contexto aislado y devuelve resumen, no todo el historial.

### Local -> CLI -> Cloud

Basado en la documentación de agent types:

- Local agent: brainstorming, debugging, tareas con feedback inmediato y contexto del editor.
- Copilot CLI: tareas bien definidas, autónomas y en background, idealmente desde un plan.
- Cloud agent: colaboración, issues, PRs y trabajo remoto orientado a revisión por equipo.

---

## Opción opcional: MCP (guía)

MCP es **opcional** en este plan. Úsalo solo si tu agente necesita capacidades externas (por ejemplo, navegador con Playwright).

### Cuándo conviene habilitar MCP

- Cuando el agente debe interactuar con navegador o recursos externos.
- Cuando necesitas tools tipo `playwright/*`.
- Cuando quieres separar claramente lógica de agente y conectores de tooling.

### Configuración opcional recomendada

Si decides usar MCP, agrega esta estructura:

```text
.vscode/
└── mcp.json
```

Ejemplo base de `.vscode/mcp.json`:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

### Ajuste opcional en `.agent.md`

- Sin MCP: usa tools built-in (`search`, `edit`, `fetch`, etc.).
- Con MCP: añade `playwright/*` (u otro `<server>/*`) en `tools:`.

---

## Plan de implementación (paso a paso)

1. Crear contexto base del proyecto: `.github/copilot-instructions.md`.
2. Añadir `.instructions.md` por dominio si el repo requiere reglas por framework o tipo de archivo.
3. Crear estructura base de agentes en `.github/agents`.
4. Crear estructura de skills en `.github/agents/skills/<skill-name>/SKILL.md` si el flujo requiere conocimiento reusable.
5. Crear prompt files en `.github/prompts/` para tareas repetibles.
6. (Opcional) Configurar MCP en `.vscode/mcp.json` si el caso de uso lo requiere.
7. Crear agentes base o agente único según el workflow.
8. Definir `tools` mínimos por rol, `agents:` permitidos y `handoffs` cuando aplique.
9. Redactar cada `SKILL.md` siguiendo la specification de Agent Skills: frontmatter válido, nombre/directorio alineados y descripción clara de activación.
10. Validar frontmatter YAML de cada `.agent.md` y `SKILL.md`.
11. Verificar aparición en Copilot Chat (`@` selector) y slash commands relevantes.
12. Probar activación real de skills, handoffs y subagentes cuando aplique.
13. Ejecutar prueba end-to-end local y, si corresponde, handoff a Copilot CLI o cloud workflow.

---

## Criterios de calidad

- Todos los `.agent.md` tienen frontmatter válido y delimitadores `---`.
- Todos los `SKILL.md` tienen frontmatter YAML válido y cuerpo Markdown útil.
- `.github/copilot-instructions.md` existe cuando el proyecto requiere reglas transversales.
- Las responsabilidades están separadas correctamente entre instrucciones, prompts, agentes y skills.
- El orquestador referencia solo agentes existentes en `agents:` y `handoffs:`.
- Si un agente define `agents:`, también define la tool `agent`.
- Cada agente tiene `description`, `tools`, `model`, `target` y alcance claros.
- Cada agente usa solo las tools necesarias para su rol.
- Cada skill tiene `name` y `description` útiles para activación por el modelo, no descripciones genéricas.
- El nombre del directorio de la skill coincide con `name`.
- Las referencias a `references/`, `scripts/` y `assets/` usan rutas relativas correctas.
- `SKILL.md` mantiene foco operativo y evita acumular documentación extensa innecesaria.
- Los workflows complejos usan handoffs o subagentes en lugar de un único agente monolítico.
- Las tareas de planning e implementation pueden separarse en sesiones distintas para evitar contaminación de contexto.
- Si se habilita MCP, `.vscode/mcp.json` es JSON válido y los servers arrancan correctamente.

---

## Checklist de verificación final

- [ ] VS Code detecta los agentes del workspace.
- [ ] Los agentes son invocables con `@`.
- [ ] Los handoffs del orquestador funcionan (Si existiese orquestador).
- [ ] Los tools configurados corresponden al rol.
- [ ] El proyecto tiene instrucciones globales y/o por dominio cuando realmente las necesita.
- [ ] No se usan agentes para sustituir instrucciones globales o prompts reutilizables.
- [ ] Las skills viven en `.github/agents/skills/` y cada una contiene `SKILL.md`.
- [ ] `name` y directorio de cada skill coinciden exactamente.
- [ ] La `description` de cada skill permite inferir claramente cuándo activarla.
- [ ] Las referencias relativas dentro de la skill resuelven correctamente.
- [ ] El contenido principal de la skill está en `SKILL.md` y el detalle extendido en `references/` o `scripts/`.
- [ ] Si hay subagentes, el coordinador limita `agents:` a los workers correctos.
- [ ] Si hay sesiones de implementación autónoma, existe criterio claro para usar local agent, Copilot CLI o cloud agent.
- [ ] (Opcional) Si hay MCP, la configuración en `.vscode/mcp.json` es válida.
- [ ] (Opcional) Si hay MCP, los tools `<server>/*` están declarados en el agente correcto.

---

## Entregables mínimos

1. `.github/copilot-instructions.md` (si el repo requiere contexto persistente)
2. `.github/agents/*.agent.md`
3. `.github/agents/skills/*/SKILL.md` (si el workspace incorpora skills)
4. `.github/prompts/*.prompt.md` (si hay tareas repetibles)
5. `README.md` con uso rápido de agentes

---

## Notas prácticas

- Mantén agentes pequeños y especializados.
- Mantén skills pequeñas, específicas y reutilizables.
- Para nuevas skills, prioriza compatibilidad con Agent Skills antes que convenciones locales cerradas.
- Si tienes disponible la librería de referencia, validar con `skills-ref validate ./mi-skill` antes de publicar o compartir una skill.
- Si dudas entre poner contenido en agente o en skill: comportamiento del rol en el agente, conocimiento reusable en la skill.
- Si dudas entre prompt file y agente: prompt para tarea repetible; agente para persona especializada con tools o handoffs.
- Si dudas entre local agent y Copilot CLI: local para iteración; CLI para ejecución autónoma bien delimitada.
- Si una tarea crece, dividir por rol y conectar por handoff.
