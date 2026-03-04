# Plan Nuevo: Bootstrap de GitHub Copilot Custom Agents

## TL;DR

Este plan define cómo preparar un workspace para crear y operar agentes personalizados de GitHub Copilot usando archivos `.agent.md`.
El foco es: **estructura de directorios**, **dependencias**, **configuración mínima**, y **checklist de validación** para que los agentes aparezcan e invoquen correctamente en VS Code.

---

## Objetivo

- Estandarizar la creación de agentes en un repositorio local.
- Dejar una base reutilizable para equipos (orquestador + subagentes).
- Mantener configuración simple, extensible y versionable.

---

## Decisiones base

| Decisión | Valor recomendado | Nota |
|---|---|---|
| Ubicación principal | `.github/agents/*.agent.md` | Compartida por equipo en repo |
| Compatibilidad Claude | `.claude/agents/*.md` | Opcional para flujos híbridos |
| Modelo default | `GPT-5 (copilot)` | Ajustable por agente |
| Convención de nombres | `dominio-rol.agent.md` | Ej. `qa-orquestador.agent.md` |

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
├── agents/
│   └── agente.agent.md
├── prompts/
│   └── agente.prompt.md
└── skills/
  └── agente/SKILL.md
README.md
```


## Plantilla estándar de agente (`.agent.md`)

```markdown
---
name: "Mi Agente"
description: "Descripción breve del agente"
tools:
  - search
  - editFiles
  - fetch
model: "GPT-5 (copilot)"
agents:
  - "*"
argument-hint: "Describe lo que necesitas..."
user-invokable: true
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

### Ubicaciones válidas

- Workspace: `.github/agents/nombre.agent.md`
- Perfil de usuario: carpeta de perfil de VS Code
- Compatibilidad Claude: `.claude/agents/nombre.md`

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

- Sin MCP: usa tools built-in (`search`, `editFiles`, `fetch`, etc.).
- Con MCP: añade `playwright/*` (u otro `<server>/*`) en `tools:`.

---

## Plan de implementación (paso a paso)

1. Crear estructura base (`.github/agents`).
2. (Opcional) Configurar MCP en `.vscode/mcp.json` si el caso de uso lo requiere.
3. Crear agentes base o agente único (depende si corresponde o no).
4. Definir `tools` mínimos por rol y handoffs del agente base.
5. Validar frontmatter YAML de cada `.agent.md`.
6. Verificar aparición en Copilot Chat (`@` selector).
7. Ejecutar prueba de invocación end-to-end entre agentes.

---

## Criterios de calidad

- Todos los `.agent.md` tienen frontmatter válido y delimitadores `---`.
- El orquestador referencia solo agentes existentes en `agents:` y `handoffs:`.
- Cada agente tiene `description`, `tools`, `model`, `target` y alcance claros.
- Si se habilita MCP, `.vscode/mcp.json` es JSON válido y los servers arrancan correctamente.

---

## Checklist de verificación final

- [ ] VS Code detecta los agentes del workspace.
- [ ] Los agentes son invocables con `@`.
- [ ] Los handoffs del orquestador funcionan (Si existiese orquestador).
- [ ] Los tools configurados corresponden al rol.
- [ ] (Opcional) Si hay MCP, la configuración en `.vscode/mcp.json` es válida.
- [ ] (Opcional) Si hay MCP, los tools `<server>/*` están declarados en el agente correcto.
---

## Entregables mínimos

1. `.github/agents/*.agent.md` 
2. `README.md` con uso rápido de agentes
---

## Notas prácticas

- Mantén agentes pequeños y especializados.
- Si una tarea crece, dividir por rol y conectar por handoff.

