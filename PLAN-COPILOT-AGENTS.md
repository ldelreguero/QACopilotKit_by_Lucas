# Guía canónica para crear agents, skills y prompts

## Índice

- [Propósito](#propósito)
- [TL;DR](#tldr)
- [Audiencia](#audiencia)
- [Doctrina del repositorio](#doctrina-del-repositorio)
- [Estructura canónica del repo](#estructura-canónica-del-repo)
- [Archivos normativos del repo](#archivos-normativos-del-repo)
- [Fuentes base](#fuentes-base)
- [Cuándo crear un agent, una skill o un prompt](#cuándo-crear-un-agent-una-skill-o-un-prompt)
- [Plantillas de diseño para agents, skills y prompts](#plantillas-de-diseño-para-agents-skills-y-prompts)
- [Patrón recomendado de arquitectura](#patrón-recomendado-de-arquitectura)
- [Decisiones específicas del repo](#decisiones-específicas-del-repo)
- [MCP en este repo](#mcp-en-este-repo)
- [Workflow recomendado para crear algo nuevo](#workflow-recomendado-para-crear-algo-nuevo)
- [Anti-Patrones que debes evitar](#anti-patrones-que-debes-evitar)
- [Criterios de calidad](#criterios-de-calidad)
- [Checklist de validación final](#checklist-de-validación-final)
- [Entregables mínimos del sistema](#entregables-mínimos-del-sistema)
- [Regla final](#regla-final)

## Propósito

Este documento es la guía de referencia para quien diseña, crea, revisa o mantiene la capa de customización de GitHub Copilot en este repositorio.

Debe usarse como fuente principal para decidir:

- cuándo crear un agent
- cuándo crear una skill
- cuándo crear un prompt
- cómo estructurar cada archivo
- cómo evitar duplicación entre agentes, skills e instrucciones
- cómo mantener consistencia con el estado real del workspace

En este repo, la separación recomendada es esta:

- [README.md](README.md): guía para el usuario final, por ejemplo un QA que quiere usar los agentes
- [PLAN-COPILOT-AGENTS.md](PLAN-COPILOT-AGENTS.md): guía para la persona que crea o mantiene agents, skills, prompts e integración MCP

## TL;DR

Si vas a extender este repositorio, sigue estas reglas primero:

1. Usa `.github/agents/*.agent.md` para agentes.
2. Usa `.github/agents/skills/<skill-name>/SKILL.md` para skills.
3. Usa `.github/prompts/*.prompt.md` para prompts reutilizables.
4. Mantén las instrucciones globales en `.github/copilot-instructions.md` y las específicas en `.github/instructions/*.instructions.md`.
5. No dupliques checklists ni estándares entre agentes y skills.
6. Si una regla es transversal del repo, no debe vivir dentro de un agent.
7. Si una capacidad es reusable, debe vivir en una skill o en `references/`, no como bloque gigante embebido en varios agentes.
8. Si un flujo es interno, usa `user-invocable: false`.
9. Si declaras `agents:`, incluye la tool `agent`.
10. Declara `model` solo si el identificador es soportado por el editor actual.

## Audiencia

Este documento está escrito para:

- quien crea nuevos agents
- quien crea nuevas skills
- quien crea prompts reutilizables
- quien revisa la arquitectura de customización del repo
- quien integra MCP o workflows especializados

No está escrito para el usuario final que solo quiere usar agentes. Ese flujo vive en [README.md](README.md).

## Doctrina del repositorio

Estas son las decisiones más importantes ya adoptadas en el workspace.

### 1. El repo se organiza por capas

Cada pieza tiene un rol distinto:

- Instrucciones globales: contexto persistente del repositorio
- Instrucciones por tipo de archivo: reglas especializadas por zona del repo
- Agents: personas o roles especializados con tools y límites claros
- Skills: conocimiento reusable, portable y enfocado
- Prompts: tareas repetibles y concretas
- MCP: acceso a tooling externo solo cuando agrega valor real

### 2. Agents pequeños, skills pequeñas, prompts concretos

La regla general es evitar archivos monolíticos.

- Un agent debe optimizar comportamiento y delegación.
- Una skill debe encapsular conocimiento reusable.
- Un prompt debe resolver una tarea repetible con foco estrecho.

### 3. Playwright tiene una base canónica compartida

En este repo, `playwright-skill` es la fuente canónica para estándares transversales Playwright.

Eso implica que agentes y skills QA deben referenciar esa base para:

- locator strategy
- waits y assertions
- organización de tests
- debugging
- quality gate

No se deben mantener checklists Playwright paralelos en varios archivos.

La fuente oficial adoptada para esa base es el repositorio `https://github.com/testdino-hq/playwright-skill`.
La instalación canónica en el workspace se realiza con:

```bash
npx skills add testdino-hq/playwright-skill
```

### 4. Los workers internos no son entrada principal del usuario

Si un workflow es especializado y no debería aparecer como opción primaria en el selector, debe definirse con:

- `user-invocable: false`

Este patrón ya se usa para workers Playwright internos como:

- `playwright-test-planner`
- `playwright-test-generator`
- `playwright-test-healer`

### 5. El catálogo visible y el catálogo interno son distintos

El usuario final no tiene por qué entender toda la topología interna.

La práctica recomendada es:

- exponer pocos agentes visibles y claros
- delegar a workers internos cuando un flujo se especializa
- documentar esa diferencia en el README de usuario y en esta guía de autoría

## Estructura canónica del repo

```text
.github/
├── copilot-instructions.md
├── instructions/
│   ├── agents.instructions.md
│   ├── prompts.instructions.md
│   └── skills.instructions.md
├── prompts/
│   └── *.prompt.md
├── agents/
│   ├── *.agent.md
│   └── skills/
│       └── <skill-name>/
│           ├── SKILL.md
│           ├── references/
│           ├── scripts/
│           └── assets/
└── workflows/

.vscode/
└── mcp.json
```

### Reglas de ubicación

- Los agents viven en `.github/agents/*.agent.md`.
- Las skills viven en `.github/agents/skills/<skill-name>/SKILL.md`.
- Los prompts viven en `.github/prompts/*.prompt.md`.
- La configuración MCP vive en `.vscode/mcp.json`.
- No crear estructuras paralelas tipo `.github/skills/` o árboles alternos fuera de esta convención.

## Archivos normativos del repo

Antes de crear o modificar cualquier pieza, revisa estos archivos:

- [.github/copilot-instructions.md](.github/copilot-instructions.md)
- [.github/instructions/agents.instructions.md](.github/instructions/agents.instructions.md)
- [.github/instructions/prompts.instructions.md](.github/instructions/prompts.instructions.md)
- [.github/instructions/skills.instructions.md](.github/instructions/skills.instructions.md)

Resumen de sus reglas:

### Reglas globales

- Mantén las definiciones pequeñas y enfocadas por responsabilidad.
- No mezcles reglas globales del proyecto dentro de un agent.
- No conviertas una skill en un prompt gigante.
- Conserva YAML frontmatter válido en `.agent.md`, `.prompt.md`, `.instructions.md` y `SKILL.md`.
- En documentación operativa, escribe en español.

### Reglas específicas para agents

- `description` debe ser específica, no genérica.
- Si defines `agents:`, incluye la tool `agent`.
- Usa `target: vscode` cuando el agent esté pensado para el editor.
- Mantén el cuerpo enfocado en comportamiento, límites, delegación y criterios de calidad.
- No conviertas el body del agent en documentación extensa del proyecto.

### Reglas específicas para skills

- Cada skill debe tener carpeta propia.
- `name` debe coincidir con el directorio y usar lowercase-hyphen.
- `description` debe explicar qué hace la skill y cuándo se activa.
- Mueve detalle extenso a `references/`, `scripts/` o `assets/`.
- Usa rutas relativas a la raíz de la skill.

### Reglas específicas para prompts

- Cada prompt debe encapsular una tarea concreta y repetible.
- Si depende de un rol especializado, referencia el agent adecuado en el frontmatter.
- No dupliques reglas ya cubiertas por instrucciones o por el propio agent.

## Fuentes base

Este plan usa GitHub Docs y VS Code Docs como base principal para `.agent.md`.
Para skills, usa la documentación oficial de Agent Skills como especificación de formato y diseño.

### GitHub Copilot

- [Create custom agents](https://docs.github.com/es/copilot/how-tos/use-copilot-agents/coding-agent/create-custom-agents)
- [About custom agents](https://docs.github.com/es/copilot/concepts/agents/coding-agent/about-custom-agents)
- [Configuring an agent profile](https://docs.github.com/es/copilot/how-tos/use-copilot-agents/coding-agent/create-custom-agents#configuring-an-agent-profile)

### VS Code Copilot

- [Customization: custom agents](https://code.visualstudio.com/docs/copilot/customization/custom-agents)
- [Copilot overview](https://code.visualstudio.com/docs/copilot/overview)
- [Concepts: tools](https://code.visualstudio.com/docs/copilot/concepts/tools)
- [Agents overview](https://code.visualstudio.com/docs/copilot/agents/overview)
- [Concepts: context](https://code.visualstudio.com/docs/copilot/concepts/context)
- [Concepts: customization](https://code.visualstudio.com/docs/copilot/concepts/customization)
- [Agents tutorial](https://code.visualstudio.com/docs/copilot/agents/agents-tutorial)
- [Agents planning](https://code.visualstudio.com/docs/copilot/agents/planning)
- [Agent tools](https://code.visualstudio.com/docs/copilot/agents/agent-tools)
- [Subagents](https://code.visualstudio.com/docs/copilot/agents/subagents)
- [Local agents](https://code.visualstudio.com/docs/copilot/agents/local-agents)
- [Copilot CLI](https://code.visualstudio.com/docs/copilot/agents/copilot-cli)
- [Context engineering guide](https://code.visualstudio.com/docs/copilot/guides/context-engineering-guide)
- [Customize Copilot guide](https://code.visualstudio.com/docs/copilot/guides/customize-copilot-guide)

### Agent Skills

- [Agent Skills](https://agentskills.io/home)
- [Specification](https://agentskills.io/specification)
- [Adding skills support](https://agentskills.io/client-implementation/adding-skills-support)

### Base canónica Playwright adoptada en este repo

- Repositorio: [testdino-hq/playwright-skill](https://github.com/testdino-hq/playwright-skill)
- Instalación: `npx skills add testdino-hq/playwright-skill`

## Cuándo crear un agent, una skill o un prompt

Usa esta tabla como criterio rápido.

| Si necesitas... | Crea... |
| --- | --- |
| un rol persistente con tools, límites y delegación | un `agent` |
| conocimiento reusable que puede activarse desde varios flujos | una `skill` |
| una tarea repetible y concreta | un `prompt` |
| reglas que deben aplicar al repo o a un tipo de archivo | `instructions` |

### Casos típicos

Crea un agent cuando:

- necesitas una persona especializada
- necesitas tools específicas
- necesitas handoffs o subagentes
- necesitas separar claramente planificación, ejecución o análisis

Crea una skill cuando:

- el conocimiento puede ser reutilizado por varios agentes
- quieres evitar duplicar procedimientos o criterios
- el contenido crecerá mejor en `references/`

Crea un prompt cuando:

- el flujo es corto y repetible
- el usuario final necesita invocarlo como tarea empaquetada
- no hace falta una nueva persona especializada

No crees un agent cuando:

- solo quieres agregar reglas globales
- el contenido es básicamente una checklist reusable
- la tarea se resuelve mejor con un prompt acotado

No crees una skill cuando:

- el contenido es específico de un único agent y no será reutilizado
- se trata de una tarea puntual de una sola ejecución

No crees un prompt cuando:

- la lógica requiere tools o restricciones propias de un rol completo
- el flujo debería ser decidido por un orquestador, no por un comando fijo

## Plantillas de diseño para agents, skills y prompts

Usa estas plantillas como punto de partida rápido. Sustituyen tanto los templates de un paso como las plantillas de diseño: la idea es que cada bloque sirva para copiar, adaptar y validar en una sola lectura.

### Plantilla unificada para agents

Úsala cuando ya decidiste que necesitas un rol nuevo.

```markdown
---
name: "mi-nuevo-agente"
description: "Rol especializado para X. Usar cuando el usuario necesite Y."
model: "GPT-5.3-Codex"
tools:
  - search
  - edit
target: vscode
user-invocable: true
disable-model-invocation: false
---

# Mi Nuevo Agente

## Rol

Eres un agente especializado en X.

## Cuándo usar

- cuando el usuario necesite Y
- cuando el flujo requiera Z

## Límites

- no hagas A
- delega B si corresponde

## Criterios de calidad

- valida C
- evita D
```

Checklist mínimo antes de guardarlo:

- el `name` es estable y claro
- la `description` dice qué hace y cuándo usarlo
- el set de tools es mínimo
- si es worker interno, usa `user-invocable: false`
- si va a delegar a otros, agrega la tool `agent`

Notas de diseño para agents:

- `model`: úsalo solo si el editor soporta ese identificador
- `agents`: si lo declaras, debes incluir la tool `agent`
- `handoffs`: útiles, pero deben apuntar a agentes que el editor realmente resuelva en ese momento
- diseña por responsabilidad, no por tecnología genérica
- si el agent crece demasiado, extrae conocimiento reusable a una skill

### Plantilla unificada para skills

Úsala cuando el conocimiento sea reusable.

```markdown
---
name: mi-nueva-skill
description: Explica qué hace la skill y cuándo debe activarse.
license: Proprietary
compatibility: Opcional si aplica.
metadata:
  author: Tu Nombre
  version: "1.0"
---

# Mi Nueva Skill

## Cuándo usar

Usa esta skill cuando...

## Resultado esperado

Qué debe dejar resuelto.

## Instrucciones

1. Haz X.
2. Verifica Y.
3. Consulta `references/...` si hace falta más detalle.

## Casos borde

- Caso 1
- Caso 2
```

Checklist mínimo antes de guardarla:

- el directorio coincide exactamente con `name`
- `description` explica activación, no solo tema general
- `SKILL.md` no está inflado con detalle que debería vivir en `references/`
- no duplica una skill existente

Notas de diseño para skills:

- `references/`: documentación adicional
- `scripts/`: automatización o helpers ejecutables cuando realmente agreguen valor
- `assets/`: plantillas o recursos reutilizables
- una skill debe ser reusable
- evita cadenas profundas de referencias
- no mezcles múltiples skills en un mismo archivo

No agregues `scripts/` o `assets/` por costumbre. Solo si resuelven una necesidad real.

### Plantilla unificada para prompts

Úsala cuando la tarea sea concreta y repetible.

```markdown
---
description: "Resuelve una tarea concreta y repetible"
agent: "qa-ejecutor"
---

Contexto breve.

Objetivo:

- hacer X
- validar Y
- devolver Z
```

Checklist mínimo antes de guardarlo:

- el prompt resuelve una sola tarea principal
- apunta al agent correcto
- no repite reglas globales ya cubiertas por instructions o por el agent
- sigue siendo legible en una sola lectura

Notas de diseño para prompts:

- deben ser cortos, reutilizables y concretos
- no deben copiar el cuerpo completo del agent
- deben servir como punto de entrada cómodo para tareas frecuentes
- si el prompt necesita demasiado contexto, probablemente no debería ser un prompt

### Regla práctica para usar las plantillas

1. Copia la plantilla más cercana.
2. Ajusta nombre, descripción y alcance.
3. Reduce tools si sobran.
4. Mueve detalle reusable a una skill o a `references/`.
5. Valida con el checklist de este documento.

## Patrón recomendado de arquitectura

Este repo favorece un patrón coordinator -> specialists -> internal workers.

### Patrón visible al usuario

- un orquestador como punto de entrada recomendado
- especialistas visibles por dominio
- README orientado al usuario final

### Patrón interno

- workers internos con `user-invocable: false`
- skills compartidas para conocimiento reusable
- prompts para atajos de tareas frecuentes

### Ejemplo real del repo

Catálogo visible:

- `qa-maestro-orquestador`
- `qa-arquitecto`
- `qa-ejecutor`
- `qa-analista`
- `qa-seguridad`
- `qa-performance`
- `qa-exploratorio`
- `Analista Universal de Logs`

Workers internos:

- `playwright-test-planner`
- `playwright-test-generator`
- `playwright-test-healer`

Esto permite que el usuario vea pocos agentes claros, mientras los workflows especializados siguen existiendo sin ensuciar el selector principal.

## Decisiones específicas del repo

### 1. README dual implícito

El repo tiene dos lecturas distintas:

- [README.md](README.md): uso del sistema por el usuario final
- [PLAN-COPILOT-AGENTS.md](PLAN-COPILOT-AGENTS.md): diseño y mantenimiento de la capa de customización

### 2. Playwright como dominio fuerte

Playwright tiene:

- agents visibles orientados a QA
- workers internos especializados
- una skill canónica transversal: `playwright-skill`
- MCP activo en `.vscode/mcp.json`

### 3. Orquestación por responsabilidad

La arquitectura QA actual ya separa:

- setup y estrategia
- ejecución y debugging generalista
- análisis de resultados
- performance
- seguridad
- exploración
- logs

No rompas esa separación sin una razón fuerte.

### 4. Los workers internos deben seguir siendo internos

Si agregas nuevos workflows especializados comparables a planner, generator o healer:

- evalúa primero si realmente deben ser visibles
- si son internos, usa `user-invocable: false`
- documenta el criterio de delegación en el orquestador y en las skills relacionadas

## MCP en este repo

MCP es opcional por diseño general, pero ya está adoptado para Playwright en [.vscode/mcp.json](.vscode/mcp.json).

### Cuándo agregar MCP nuevo

Agrega un server MCP solo si:

- expone una capacidad externa real que no existe con tools built-in
- resuelve un flujo frecuente o de alto valor
- su coste operativo y de mantenimiento está justificado

No agregues MCP solo por novedad o por duplicar algo que ya resuelve una tool built-in.

### Reglas de integración MCP

- Mantén la configuración en `.vscode/mcp.json`.
- Declara tools específicas en el agent correcto.
- Evita dar acceso a tooling externo a agents que no lo necesitan.
- Si el flujo es interno, considera un worker en vez de inflar un agent visible.

## Workflow recomendado para crear algo nuevo

### Si quieres crear un nuevo agent

1. Define el problema exacto.
2. Decide si de verdad necesita ser un agent y no una skill o un prompt.
3. Define su audiencia: visible o interno.
4. Define tools mínimas.
5. Decide si necesita `agents:` o `handoffs:`.
6. Escribe un body corto, enfocado en comportamiento y límites.
7. Valida frontmatter y aparición en el editor.

### Si quieres crear una nueva skill

1. Confirma que el conocimiento será reusable.
2. Crea carpeta propia bajo `.github/agents/skills/`.
3. Alinea `name` con el directorio.
4. Mantén `SKILL.md` corto y operativo.
5. Mueve detalle a `references/`.
6. Revisa que no esté duplicando otra skill existente.

### Si quieres crear un nuevo prompt

1. Verifica que la tarea sea repetible y concreta.
2. Reutiliza el agent adecuado en el frontmatter.
3. Mantén el prompt corto.
4. No copies reglas que ya viven en instructions o en el agent.

## Anti-Patrones que debes evitar

- meter reglas globales del repo dentro de un agent
- duplicar el mismo estándar en varios agents y skills
- crear agents genéricos con demasiadas tools
- usar un agent cuando bastaba un prompt
- usar una skill para una tarea de un solo uso
- referenciar modelos no soportados por el editor actual
- dejar workers internos visibles al usuario sin una razón clara
- convertir el README de usuario en documento de arquitectura interna
- convertir el plan de arquitectura en documento para el usuario final

## Criterios de calidad

Una entrega está bien si cumple esto:

- todos los `.agent.md` tienen frontmatter válido
- todos los `SKILL.md` tienen frontmatter válido y foco operativo
- las instrucciones globales y por tipo de archivo siguen siendo la autoridad normativa
- cada agent tiene un alcance claro y un set de tools defensible
- las skills no duplican conocimiento ya cubierto por otras skills o por `playwright-skill`
- los prompts son concretos y no monolíticos
- los workers internos usan `user-invocable: false`
- el README de usuario y este plan no compiten entre sí, sino que se complementan

## Checklist de validación final

- [ ] El nuevo archivo está en la ruta correcta.
- [ ] El frontmatter es válido.
- [ ] `description` explica con claridad qué hace y cuándo usarlo.
- [ ] Si hay `agents:`, también está la tool `agent`.
- [ ] Si el agent es interno, usa `user-invocable: false`.
- [ ] Si se declara `model`, el identificador es soportado por el editor actual.
- [ ] El body del agent está enfocado y no es un manual enciclopédico.
- [ ] El `name` de la skill coincide con su directorio.
- [ ] Las referencias internas usan rutas relativas correctas.
- [ ] No se está duplicando contenido que debería vivir en instructions o en una skill existente.
- [ ] Si se añadió MCP, la configuración está justificada y válida.
- [ ] El cambio mejora el catálogo sin aumentar ambigüedad innecesaria.

## Entregables mínimos del sistema

1. [.github/copilot-instructions.md](.github/copilot-instructions.md)
2. `.github/agents/*.agent.md`
3. `.github/agents/skills/*/SKILL.md`
4. `.github/prompts/*.prompt.md`
5. [README.md](README.md) orientado al usuario final
6. [PLAN-COPILOT-AGENTS.md](PLAN-COPILOT-AGENTS.md) orientado a quien crea y mantiene la customización

## Regla final

Si dudas dónde poner algo, decide así:

1. Regla transversal del repo: instructions.
2. Rol especializado con tools y límites: agent.
3. Conocimiento reusable: skill.
4. Tarea repetible y concreta: prompt.
5. Flujo especializado que no debe ser visible al usuario: worker interno.

Si una pieza nueva no mejora claridad, mantenibilidad o separación de responsabilidades, no debería entrar al repo todavía.
