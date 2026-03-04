# SKILL: QA Ejecutor

> Funciones y comandos para implementación de tests, ejecución, debugging con MCP y self-healing de selectores.

---

## Funciones de Implementación

### `generateTest(spec)`
Genera un test basado en una especificación natural del usuario.

**Proceso:**
```
1. Analizar la spec del usuario
2. Determinar tipo: UI / API / Hybrid
3. Usar MCP para inspeccionar la página/endpoint si es necesario
4. Generar código del test siguiendo patrones del proyecto
5. Guardar en la ubicación correcta dentro de tests/
```

**Ejemplo de uso con MCP:**
```
// El agente debe:
// 1. Usar browser_navigate para ir a la página objetivo
// 2. Usar browser_snapshot para entender la estructura
// 3. Usar browser_generate_locator para obtener selectores estables
// 4. Generar el test usando esos selectores verificados
```

---

### `findStableSelectors(url, elements)`
Usa Playwright MCP para encontrar selectores estables para elementos de una página.

**Flujo MCP:**
```
Paso 1: browser_navigate → url
Paso 2: browser_snapshot → obtener accessibility tree
Paso 3: Para cada elemento:
  a. browser_generate_locator → obtener locator recomendado
  b. browser_verify_element_visible → confirmar visibilidad
  c. Si falla → intentar con browser_evaluate para inspeccionar DOM
Paso 4: Retornar mapa de selectores verificados
```

**Prioridad de selectores (de más a menos estable):**
```
1. data-testid="..."           → Más estable, requiere colaboración con devs
2. getByRole('button', {name}) → Semántico, resistente a cambios de markup
3. getByLabel('...')           → Ideal para inputs de formulario
4. getByText('...')            → Bueno para contenido estático
5. getByPlaceholder('...')     → Alternativa para inputs
6. css=[data-*]               → Atributos data personalizados
7. css=.class                  → Frágil, evitar si posible
8. xpath=//...                 → Último recurso, muy frágil
```

---

### `healBrokenSelector(testFile, failedSelector)`
Repara un selector que ha dejado de funcionar usando MCP.

**Flujo de Self-Healing:**
```bash
# Paso 1: Navegar a la página del test fallido
# → MCP: browser_navigate

# Paso 2: Capturar snapshot del estado actual
# → MCP: browser_snapshot

# Paso 3: Buscar el elemento con el selector roto
# → MCP: browser_generate_locator (para el elemento visual más cercano)

# Paso 4: Verificar el nuevo selector
# → MCP: browser_verify_element_visible

# Paso 5: Actualizar el test file
# → Tool: editFiles

# Paso 6: Ejecutar para confirmar
# → Terminal: npx playwright test <testFile>
```

**Ejemplo de código de self-healing utility:**
```javascript
// utils/selector-healer.js

/**
 * Intenta múltiples estrategias de localización para un elemento
 * @param {import('@playwright/test').Page} page
 * @param {Object} strategies - Estrategias de localización
 * @returns {import('@playwright/test').Locator}
 */
export async function resilientLocator(page, strategies) {
  const { testId, role, label, text, css } = strategies;

  // Intentar en orden de prioridad
  if (testId) {
    const loc = page.getByTestId(testId);
    if (await loc.isVisible().catch(() => false)) return loc;
  }

  if (role) {
    const loc = page.getByRole(role.type, { name: role.name });
    if (await loc.isVisible().catch(() => false)) return loc;
  }

  if (label) {
    const loc = page.getByLabel(label);
    if (await loc.isVisible().catch(() => false)) return loc;
  }

  if (text) {
    const loc = page.getByText(text);
    if (await loc.isVisible().catch(() => false)) return loc;
  }

  if (css) {
    const loc = page.locator(css);
    if (await loc.isVisible().catch(() => false)) return loc;
  }

  throw new Error(`No se encontró el elemento con ninguna estrategia: ${JSON.stringify(strategies)}`);
}
```

---

### `runTests(options)`
Ejecuta tests con diferentes configuraciones.

```bash
# ═══════════════════════════════════════════
# EJECUCIÓN DE TESTS
# ═══════════════════════════════════════════

# Ejecutar todos los tests
npx playwright test

# Ejecutar solo tests UI (con browser)
npx playwright test --project=ui

# Ejecutar solo tests API (sin browser)
npx playwright test --project=api

# Ejecutar un archivo específico
npx playwright test tests/ui/auth/login.spec.js

# Ejecutar tests que coincidan con un patrón
npx playwright test -g "login"

# Ejecutar con browser visible (para debugging visual)
npx playwright test --headed

# Ejecutar con debug mode (pausa en cada step)
npx playwright test --debug

# Ejecutar con trace activado
npx playwright test --trace on

# Ejecutar con retries
npx playwright test --retries=3

# Ejecutar en un worker
npx playwright test --workers=1

# ═══════════════════════════════════════════
# EJECUCIÓN CON CLI
# ═══════════════════════════════════════════

# Abrir URL en browser controlado por CLI
playwright-cli open http://localhost:3000

# Abrir con browser específico
playwright-cli open --browser=firefox http://localhost:3000

# Abrir en modo headed (con ventana visible)
playwright-cli open --headed http://localhost:3000

# Navegar a una URL en sesión existente
playwright-cli goto http://localhost:3000/login

# Capturar snapshot del estado actual
playwright-cli snapshot

# Click en un elemento por referencia
playwright-cli click <ref>

# Llenar un campo
playwright-cli fill <ref> "texto de prueba"
```

---

### `debugWithMCP(testFile, errorMessage)`
Debugging de tests fallidos usando Playwright MCP en tiempo real.

**Flujo de debugging:**
```
PASO 1 — Reproducir el escenario
├── browser_navigate → URL del test fallido
├── Ejecutar los pasos previos al fallo manualmente via MCP
└── Llegar al punto exacto del error

PASO 2 — Inspeccionar estado
├── browser_snapshot → Ver accessibility tree actual
├── browser_take_screenshot → Captura visual
├── browser_console_messages → Errores de JavaScript
└── browser_network_requests → Requests fallidos

PASO 3 — Diagnosticar
├── ¿El elemento existe? → browser_verify_element_visible
├── ¿El selector es correcto? → browser_generate_locator
├── ¿Hay errores JS? → browser_console_messages
├── ¿La API respondió correctamente? → browser_network_requests
└── ¿El timing es un problema? → browser_wait_for

PASO 4 — Corregir
├── Actualizar selector si es necesario
├── Añadir waits si hay timing issues
├── Corregir assertions si los datos cambiaron
└── Actualizar fixtures si las precondiciones cambiaron

PASO 5 — Verificar
├── npx playwright test <testFile> → Re-ejecutar
└── Si pasa → Fix confirmado, reportar
```

---

### `codegenFlow(url)`
Genera tests automáticamente grabando interacciones del usuario.

```bash
# ═══════════════════════════════════════════
# CODEGEN — Grabación de Tests
# ═══════════════════════════════════════════

# Abrir codegen para una URL
npx playwright codegen http://localhost:3000

# Codegen con viewport específico
npx playwright codegen --viewport-size=1280,720 http://localhost:3000

# Codegen con emulación de dispositivo
npx playwright codegen --device="iPhone 14" http://localhost:3000

# Codegen guardando resultado en archivo
npx playwright codegen --output=tests/ui/generated-test.spec.js http://localhost:3000

# Codegen con estado de autenticación previo
npx playwright codegen --load-storage=auth-state.json http://localhost:3000

# Codegen para guardar estado de autenticación
npx playwright codegen --save-storage=auth-state.json http://localhost:3000/login

# ═══════════════════════════════════════════
# CODEGEN VIA MCP (modo agente)
# ═══════════════════════════════════════════

# El servidor MCP también soporta modo codegen:
# npx @playwright/mcp@latest --codegen
# Esto genera código Playwright automáticamente basado en las acciones MCP
```

---

## Patrones de Tests Reutilizables

### Page Object Model (POM) base
```javascript
// utils/pages/login.page.js

export class LoginPage {
  constructor(page) {
    this.page = page;
    this.emailInput = page.getByLabel('Email');
    this.passwordInput = page.getByLabel('Password');
    this.submitButton = page.getByRole('button', { name: /sign in|log in/i });
    this.errorMessage = page.getByRole('alert');
  }

  async goto() {
    await this.page.goto('/login');
  }

  async login(email, password) {
    await this.emailInput.fill(email);
    await this.passwordInput.fill(password);
    await this.submitButton.click();
  }

  async getError() {
    return this.errorMessage.textContent();
  }
}
```

### API Request Helper
```javascript
// utils/api-client.js

export class APIClient {
  constructor(request) {
    this.request = request;
  }

  async get(endpoint, options = {}) {
    const response = await this.request.get(endpoint, options);
    return {
      status: response.status(),
      headers: response.headers(),
      body: await response.json().catch(() => null),
      ok: response.ok(),
    };
  }

  async post(endpoint, data, options = {}) {
    const response = await this.request.post(endpoint, { data, ...options });
    return {
      status: response.status(),
      headers: response.headers(),
      body: await response.json().catch(() => null),
      ok: response.ok(),
    };
  }

  async put(endpoint, data, options = {}) {
    const response = await this.request.put(endpoint, { data, ...options });
    return {
      status: response.status(),
      headers: response.headers(),
      body: await response.json().catch(() => null),
      ok: response.ok(),
    };
  }

  async delete(endpoint, options = {}) {
    const response = await this.request.delete(endpoint, options);
    return {
      status: response.status(),
      headers: response.headers(),
      body: await response.json().catch(() => null),
      ok: response.ok(),
    };
  }
}
```

---

## Comandos de Debugging

```bash
# Mostrar reporte HTML de la última ejecución
npx playwright show-report

# Abrir trace de un test específico
npx playwright show-trace test-results/trace.zip

# Ver screenshots de fallos
# (Se guardan automáticamente en test-results/)

# Dashboard visual del CLI
playwright-cli show

# Consola del navegador desde CLI
playwright-cli console

# Red del navegador desde CLI
playwright-cli network
```
