# SKILL: QA Maestro Orquestador

> Funciones y comandos disponibles para el agente orquestador principal.

---

## Funciones de Orquestación

### `startMCPServer()`
Inicia el servidor Playwright MCP para que los agentes puedan controlar el navegador.

**Modo stdio (default — VS Code lo gestiona automáticamente):**
```bash
# No requiere inicio manual. VS Code inicia el servidor MCP
# automáticamente mediante la configuración en .vscode/mcp.json
```

**Modo HTTP/SSE (para debugging o acceso externo):**
```bash
npx @playwright/mcp@latest --port 8931
# Accesible en: http://localhost:8931/mcp
```

**Con opciones avanzadas:**
```bash
# Con browser específico y headed mode
npx @playwright/mcp@latest --port 8931 --browser=chromium --headless=false --caps=testing,vision,pdf

# Con viewport personalizado
npx @playwright/mcp@latest --port 8931 --viewport-size="1920,1080"

# Con estado de sesión previo
npx @playwright/mcp@latest --port 8931 --storage-state=auth-state.json
```

---

### `checkProjectHealth()`
Verifica que el proyecto está correctamente configurado para ejecutar tests.

```bash
# 1. Verificar que Node.js está disponible
node --version

# 2. Verificar que las dependencias están instaladas
npm ls @playwright/test @playwright/mcp @playwright/cli 2>&1

# 3. Verificar que los browsers están instalados
npx playwright install --dry-run

# 4. Verificar versiones
npx playwright --version
playwright-cli --version 2>&1 || echo "playwright-cli no instalado"

# 5. Verificar configuración de Playwright
node -e "try { require('./playwright.config.js'); console.log('Config OK') } catch(e) { console.error('Config ERROR:', e.message) }"

# 6. Verificar estructura de carpetas
# (Usar herramienta 'search' o 'codebase' para listar)
```

**Checklist de salud del proyecto:**
```
✅/❌ package.json existe
✅/❌ node_modules/ existe (dependencias instaladas)
✅/❌ playwright.config.js existe y es válido
✅/❌ .vscode/mcp.json existe y configura playwright
✅/❌ .env existe (o .env.example como referencia)
✅/❌ Carpeta tests/ existe con al menos un test
✅/❌ Browsers de Playwright instalados
✅/❌ @playwright/cli accesible
```

---

### `delegateToAgent(agentName, context)`
Lógica de delegación a subagentes. El orchestrator usa esta lógica mental para decidir qué agente invocar.

**Tabla de decisión:**

| Señal en la solicitud | Agente destino | Contexto a proporcionar |
|---|---|---|
| "configurar", "setup", "inicializar", "crear proyecto" | `@qa-arquitecto` | Tipo de proyecto (Web/API/Híbrido) |
| "planificar", "estrategia", "diseñar tests" | `@qa-arquitecto` | Requisitos del sistema bajo test |
| "implementar test", "codear", "escribir test" | `@qa-ejecutor` | Spec del test + URL/endpoint |
| "ejecutar", "correr tests", "run" | `@qa-ejecutor` | Filtros (proyecto, archivo, tag) |
| "debuggear", "arreglar test", "falla" | `@qa-ejecutor` | Error message + test file |
| "generar selectores", "codegen" | `@qa-ejecutor` | URL de la página |
| "analizar", "comparar", "reportar" | `@qa-analista` | Datos a comparar + baseline |
| "regresión", "diff", "cambios" | `@qa-analista` | Versiones a comparar |
| "reporte allure", "resultados" | `@qa-analista` | Path de allure-results |
| "pipeline completo", "todo" | Secuencial | Todos → architect → executor → analyst |

---

### `runFullPipeline(config)`
Ejecuta el pipeline completo de QA delegando a cada agente en secuencia.

**Flujo:**
```
Fase 1: SETUP (@qa-arquitecto)
├── Verificar project health
├── Crear/actualizar configuración si es necesario
└── Instalar dependencias faltantes

Fase 2: EXECUTE (@qa-ejecutor)
├── Ejecutar suite de tests completa
├── Capturar resultados en allure-results/
└── Debugging automático de fallos

Fase 3: ANALYZE (@qa-analista)
├── Generar reporte Allure
├── Comparar con baseline anterior (si existe)
├── Identificar regresiones
└── Producir reporte final con recomendaciones
```

**Comando combinado para pipeline rápido:**
```bash
# Ejecutar tests con Allure reporter y generar reporte
npx playwright test --reporter=allure-playwright && npx allure generate allure-results --clean -o allure-report
```

---

### `listActiveSessions()`
Lista las sesiones activas del CLI de Playwright.

```bash
# Listar sesiones activas del CLI
playwright-cli list

# Cerrar todas las sesiones
playwright-cli close-all

# Forzar cierre de todas las sesiones
playwright-cli kill-all
```

---

## Comandos MCP Disponibles (via .vscode/mcp.json)

El orchestrator puede usar todas las herramientas MCP declaradas con `playwright/*`:

| Herramienta | Uso del Orchestrator |
|---|---|
| `browser_navigate` | Navegar a URL para inspección rápida |
| `browser_snapshot` | Verificar estado de una página |
| `browser_take_screenshot` | Capturar evidencia visual |
| `browser_console_messages` | Revisar errores de consola |
| `browser_network_requests` | Inspeccionar tráfico de red |
| `browser_tabs` | Gestionar pestañas abiertas |
| `browser_close` | Cerrar navegador después de inspección |

> **Nota**: El orchestrator usa MCP **solo para inspección rápida**. La ejecución de tests se delega siempre a `@qa-ejecutor`.
