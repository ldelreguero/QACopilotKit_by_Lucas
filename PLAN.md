# Plan: Sistema Jerárquico QA Agents con Playwright MCP

## TL;DR

Se generarán **16 archivos** desde cero en un workspace vacío: 4 agentes (`.agent.md`), 4 skills (`SKILL.md`), 4 prompts (`.prompt.md`), más configuración auxiliar (`.vscode/mcp.json`, `package.json`). Los agentes operan en **VS Code local**, usan **Playwright MCP** (`@playwright/mcp`) para control de navegador en tiempo real y **ambos CLIs** (`@playwright/cli` + `npx playwright`) para codegen, testing y automatización por agentes. Modelo por defecto: **Claude Sonnet 4** (orchestrator usa **Claude Opus 4** por su necesidad de razonamiento complejo), fácilmente modificable en el frontmatter de cada archivo.

---

## Decisiones Técnicas

| Decisión | Valor | Justificación |
|---|---|---|
| **Modelo orchestrator** | Claude Opus 4 (copilot) | Requiere razonamiento complejo para orquestar; los demás usan Claude Sonnet 4 (copilot) para equilibrio velocidad/calidad |
| **Target** | VS Code local | MCP servers en `.vscode/mcp.json`, no inline en frontmatter |
| **CLI** | Ambos | `@playwright/cli` (agentes) + `npx playwright` (test runner/codegen) se integran como complementarios |
| **Caps MCP** | `testing,vision,pdf` | Habilitados por defecto para máxima capacidad de los agentes |
| **Idioma** | Bilingüe | Definiciones/config en inglés, instrucciones y respuestas en español |

---

## Investigación Previa

### 1. Playwright MCP Server

**Paquete npm:** `@playwright/mcp`
**Instalación:** `npx @playwright/mcp@latest`

**Configuración JSON estándar (stdio):**
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

**Transporte:**
- **stdio** (default): el cliente MCP lanza el proceso directamente.
- **HTTP/SSE**: con `--port 8931`, luego `"url": "http://localhost:8931/mcp"`.
- **Programático**: `createConnection()` + `SSEServerTransport`.

**Herramientas expuestas (Core automation):**

| Herramienta | Descripción | Read-only |
|---|---|---|
| `browser_navigate` | Navegar a URL | false |
| `browser_click` | Click en elemento (ref) | false |
| `browser_type` | Escribir texto en elemento | false |
| `browser_snapshot` | Capturar accessibility snapshot | true |
| `browser_take_screenshot` | Capturar screenshot | true |
| `browser_hover` | Hover sobre elemento | false |
| `browser_select_option` | Seleccionar opción en dropdown | false |
| `browser_press_key` | Presionar tecla | false |
| `browser_navigate_back` | Ir atrás | false |
| `browser_drag` | Drag and drop | false |
| `browser_fill_form` | Llenar múltiples campos | false |
| `browser_file_upload` | Subir archivos | false |
| `browser_handle_dialog` | Manejar diálogos | false |
| `browser_evaluate` | Ejecutar JS en página | false |
| `browser_run_code` | Ejecutar snippet Playwright | false |
| `browser_wait_for` | Esperar texto/tiempo | false |
| `browser_close` | Cerrar página | false |
| `browser_resize` | Redimensionar ventana | false |
| `browser_console_messages` | Obtener mensajes de consola | true |
| `browser_network_requests` | Listar peticiones de red | true |

**Tab management:** `browser_tabs` (list/create/close/select)
**Browser installation:** `browser_install`

**Opt-in capabilities** (`--caps=`):
- **vision**: `browser_mouse_click_xy`, `browser_mouse_drag_xy`, `browser_mouse_move_xy`, `browser_mouse_down/up`, `browser_mouse_wheel`
- **pdf**: `browser_pdf_save`
- **testing**: `browser_generate_locator`, `browser_verify_element_visible`, `browser_verify_text_visible`, `browser_verify_list_visible`, `browser_verify_value`
- **tracing**: herramientas de tracing

**Opciones de configuración clave:** `--browser`, `--headless`, `--caps`, `--codegen`, `--viewport-size`, `--device`, `--isolated`, `--port`, `--config`, `--storage-state`, `--user-data-dir`, `--init-page`, `--init-script`.

### 2. Playwright CLI

**Paquete npm:** `@playwright/cli`
**Instalación:**
```bash
npm install -g @playwright/cli@latest
playwright-cli --help
```

> **Nota:** Este es un proyecto **diferente** de `npx playwright` (que es el runner de Playwright Test). Este CLI es específico para automatización por agentes coding.

**Comandos principales:**

| Categoría | Comandos |
|---|---|
| **Core** | `open [url]`, `goto <url>`, `close`, `type <text>`, `click <ref>`, `fill <ref> <text>`, `drag`, `hover <ref>`, `select <ref> <val>`, `upload`, `check/uncheck <ref>`, `snapshot`, `eval <func>`, `dialog-accept`, `dialog-dismiss`, `resize` |
| **Navegación** | `go-back`, `go-forward`, `reload` |
| **Teclado** | `press <key>`, `keydown <key>`, `keyup <key>` |
| **Mouse** | `mousemove <x> <y>`, `mousedown`, `mouseup`, `mousewheel` |
| **Capturas** | `screenshot [ref]`, `pdf` |
| **Tabs** | `tab-list`, `tab-new`, `tab-close`, `tab-select` |
| **Storage** | `state-save`, `state-load`, `cookie-*`, `localstorage-*`, `sessionstorage-*` |
| **Network** | `route`, `route-list`, `unroute` |
| **DevTools** | `console`, `network`, `run-code`, `tracing-start/stop`, `video-start/stop` |
| **Sesiones** | `list`, `close-all`, `kill-all`, `-s=name <cmd>` |
| **Monitoring** | `show` (dashboard visual) |

**Open params:** `--browser=chrome`, `--headed`, `--persistent`, `--extension`, `--config=file.json`
**Snapshots automáticos** tras cada comando, guardados como `.yml`.

### 3. GitHub Copilot Custom Agents (`.agent.md`)

**Ubicación de archivos:**
- **Workspace:** `.github/agents/nombre.agent.md`
- **User profile:** carpeta del perfil actual
- **Claude compat:** `.claude/agents/nombre.md`

**Estructura del archivo `.agent.md`:**

```markdown
---
name: "Mi Agente"
description: "Descripción breve del agente"
tools:
  - search
  - editFiles
  - playwright/*
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
    model: "Claude Sonnet 4.5 (copilot)"
---

# Instrucciones del agente

Aquí van las instrucciones en Markdown...
```

**Campos YAML frontmatter:**

| Campo | Tipo | Descripción |
|---|---|---|
| `name` | string | Nombre del agente (default: nombre del archivo) |
| `description` | string | Descripción, se muestra como placeholder en chat |
| `argument-hint` | string | Hint en el campo de input |
| `tools` | string[] | Lista de herramientas/tool sets. Formato MCP: `<server>/*` |
| `agents` | string[] | Subagentes permitidos. `*` = todos, `[]` = ninguno |
| `model` | string \| string[] | Modelo(s) a usar. Array = fallback priorizado |
| `user-invokable` | boolean | Mostrar en dropdown (default: true) |
| `disable-model-invocation` | boolean | Impedir invocación como subagente (default: false) |
| `target` | string | `vscode` o `github-copilot` |
| `mcp-servers` | list | Config JSON de servidores MCP (solo para `target: github-copilot`) |
| `handoffs` | list | Transiciones guiadas entre agentes |

---

## Pasos de Implementación

### Paso 1 — Configuración base del proyecto

**Archivo 1: `package.json`**
- Dependencias: `@playwright/test`, `@playwright/mcp`, `@playwright/cli`, `dotenv`, `@faker-js/faker`, `allure-playwright`, `allure-commandline`, `prettier`
- Scripts: `test`, `test:api`, `test:ui`, `codegen`, `report`, `mcp:start`, `cli:open`

**Archivo 2: `.vscode/mcp.json`**
- Servidor `playwright` via stdio: `npx @playwright/mcp@latest`
- Caps habilitadas: `testing`, `vision`, `pdf`
- Browser: `chromium` (por defecto), modo headless configurable

---

### Paso 2 — Los 4 archivos `.agent.md`

**Archivo 3: `.github/agents/qa-maestro-orquestador.agent.md`**
- **YAML**: `name`, `description`, `tools: [playwright/*, editFiles, search, fetch, terminalLastCommand, runInTerminal, codebase]`, `model: "Claude Opus 4 (copilot)"`, `agents: ["qa-arquitecto", "qa-ejecutor", "qa-analista"]`, `user-invokable: true`
- **Handoffs**: 4 transiciones → cada subagente con labels descriptivos (Setup, Execute, Analyze, Full Pipeline)
- **Cuerpo**: Instrucciones de orquestación, lógica de decisión sobre cuándo usar MCP vs CLI, flujo de puntos 1-8, reglas de bilingüismo, Chain of Thought para decidir el agente correcto

**Archivo 4: `.github/agents/qa-arquitecto.agent.md`**
- **YAML**: `tools: [playwright/*, editFiles, search, runInTerminal, fetch]`, `model: "Claude Sonnet 4 (copilot)"`, `agents: []`, `user-invokable: true`
- **Alcance**: Puntos 1, 2, 5, 6 — setup de proyecto, estructura de carpetas, `playwright.config.js`, configuración MCP, diseño de estrategia de tests API/UI
- **Cuerpo**: Instrucciones para crear `playwright.config.js`, `jsconfig.json`, estructura `tests/ui/`, `tests/api/`, `fixtures/`, `utils/`, manejo de `.env`, configuración de Allure

**Archivo 5: `.github/agents/qa-ejecutor.agent.md`**
- **YAML**: `tools: [playwright/*, editFiles, search, runInTerminal, terminalLastCommand, codebase]`, `model: "Claude Sonnet 4 (copilot)"`, `agents: []`, `user-invokable: true`
- **Alcance**: Puntos 3, 7 — implementación de tests, debugging con MCP, codegen con CLI
- **Cuerpo**: Instrucciones para usar `browser_snapshot` + `browser_generate_locator` para self-healing selectors, `playwright codegen` para grabar flujos, `playwright-cli snapshot` para inspección, ejecución headless vía `npx playwright test`, debugging con `browser_console_messages` y `browser_network_requests`

**Archivo 6: `.github/agents/qa-analista.agent.md`**
- **YAML**: `tools: [playwright/*, editFiles, search, runInTerminal, fetch, codebase]`, `model: "Claude Sonnet 4 (copilot)"`, `agents: []`, `user-invokable: true`
- **Alcance**: Puntos 4, 8 — análisis de datos, JSON diffing, snapshots, reportes Allure
- **Cuerpo**: Instrucciones para comparar JSON responses, deep diffing, regresión histórica, generación de reportes con `allure generate`, análisis de `browser_network_requests`, validación de integridad de datos

---

### Paso 3 — Los 4 archivos `SKILL.md`

**Archivo 7: `.github/skills/qa-maestro-orquestador/SKILL.md`**
- Funciones: `startMCPServer()`, `runFullPipeline()`, `delegateToAgent()`, `checkProjectHealth()`
- Comandos: `npx @playwright/mcp@latest --port 8931`, `playwright-cli list`

**Archivo 8: `.github/skills/qa-arquitecto/SKILL.md`**
- Funciones: `scaffoldProject()`, `configurePlaywright()`, `setupMCP()`, `createAPITestStructure()`, `createUITestStructure()`, `installDependencies()`
- Comandos: `npm init -y`, `npm install`, `npx playwright install`, `playwright-cli install --skills`

**Archivo 9: `.github/skills/qa-ejecutor/SKILL.md`**
- Funciones: `generateTest()`, `findStableSelectors()`, `runTests()`, `debugWithMCP()`, `codegenFlow()`, `healBrokenSelector()`
- Comandos: `npx playwright codegen <url>`, `npx playwright test`, `playwright-cli open <url>`, `playwright-cli snapshot`, `playwright-cli click <ref>`

**Archivo 10: `.github/skills/qa-analista/SKILL.md`**
- Funciones: `compareJSON()`, `deepDiff()`, `generateAllureReport()`, `analyzeNetworkLogs()`, `regressionCheck()`, `snapshotCompare()`
- Comandos: `npx allure generate`, `npx allure open`, `npx playwright test --reporter=allure-playwright`

---

### Paso 4 — Los 4 archivos `.prompt.md`

**Archivo 11: `.github/prompts/qa-maestro-orquestador.prompt.md`**
- System prompt con Chain of Thought: evaluar solicitud → determinar bloque (A o B) → seleccionar punto(s) → delegar al agente correcto → verificar resultado
- Reglas: bilingüismo, preguntar antes de instalar, nunca ejecutar sin plan

**Archivo 12: `.github/prompts/qa-arquitecto.prompt.md`**
- System prompt con CoT: analizar requisitos → verificar existencia de config → crear/actualizar estructura → validar con dry-run
- Incluye templates de `playwright.config.js` y estructura de carpetas

**Archivo 13: `.github/prompts/qa-ejecutor.prompt.md`**
- System prompt con CoT: recibir spec → usar MCP snapshot para entender página → generar locators estables → implementar test → ejecutar → debugging si falla
- Flujo de self-healing: snapshot → locate → validate → fallback

**Archivo 14: `.github/prompts/qa-analista.prompt.md`**
- System prompt con CoT: recopilar datos → comparar → identificar anomalías → generar reporte → recomendar acciones
- Plantillas de output para JSON diff y reportes de regresión

---

## Estructura Final del Workspace

```
.github/
├── agents/
│   ├── qa-maestro-orquestador.agent.md    # Archivo 3
│   ├── qa-arquitecto.agent.md              # Archivo 4
│   ├── qa-ejecutor.agent.md               # Archivo 5
│   └── qa-analista.agent.md                # Archivo 6
├── skills/
│   ├── qa-maestro-orquestador/
│   │   └── SKILL.md                       # Archivo 7
│   ├── qa-arquitecto/
│   │   └── SKILL.md                       # Archivo 8
│   ├── qa-ejecutor/
│   │   └── SKILL.md                       # Archivo 9
│   └── qa-analista/
│       └── SKILL.md                       # Archivo 10
└── prompts/
    ├── qa-maestro-orquestador.prompt.md   # Archivo 11
    ├── qa-arquitecto.prompt.md             # Archivo 12
    ├── qa-ejecutor.prompt.md              # Archivo 13
    └── qa-analista.prompt.md               # Archivo 14
.vscode/
└── mcp.json                               # Archivo 2
package.json                               # Archivo 1
```

**Total: 16 archivos** (14 numerados + 2 de configuración base)

---

## Matriz de Puntos por Agente

| Punto | Bloque | Descripción | Agente Responsable |
|-------|--------|-------------|-------------------|
| 1 | A - Web & API | Inicio Web: Setup Playwright + MCP Server | `@qa-arquitecto` |
| 2 | A - Web & API | Planificación API Híbrida: Tests API de soporte para UI | `@qa-arquitecto` |
| 3 | A - Web & API | Ejecución y Debugging Híbrida: CLI + MCP en vivo | `@qa-ejecutor` |
| 4 | A - Web & API | Análisis de Datos Híbrida: Responses + históricos | `@qa-analista` |
| 5 | B - API Pura | Inicio API Solo: Setup sin browsers | `@qa-arquitecto` |
| 6 | B - API Pura | Planificación API Pura: Contratos + performance | `@qa-arquitecto` |
| 7 | B - API Pura | Ejecución y Debugging Pura: Headless + payloads | `@qa-ejecutor` |
| 8 | B - API Pura | Análisis Avanzado Pura: Deep diffing + regresión | `@qa-analista` |

---

## Verificación Post-Implementación

1. **Sintaxis YAML**: Validar que cada `.agent.md` tiene frontmatter YAML bien formado con `---` delimitadores
2. **Referencias cruzadas**: Verificar que el orchestrator referencia correctamente los 3 subagentes en `agents:` y `handoffs:`
3. **Tools**: Confirmar que `playwright/*` está presente en todos los agentes y que las herramientas adicionales (`editFiles`, `runInTerminal`, etc.) corresponden a los built-ins de VS Code
4. **MCP Config**: Verificar que `.vscode/mcp.json` usa la sintaxis correcta de `inputs` + `mcpServers`
5. **Package.json**: Ejecutar `npm install` y confirmar que todas las dependencias resuelven
6. **Agente invocable**: Abrir VS Code Copilot Chat → verificar que los 4 agentes aparecen en el dropdown con `@`
