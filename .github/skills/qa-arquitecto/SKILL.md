# SKILL: QA Arquitecto

> Funciones y comandos para configuración de proyectos, estructura de tests y planificación de estrategias QA.

---

## Funciones de Setup

### `scaffoldProject(type)`
Crea la estructura completa del proyecto desde cero.

**Parámetro `type`**: `"web"` | `"api"` | `"hybrid"`

```bash
# 1. Inicializar proyecto Node.js
npm init -y

# 2. Instalar dependencias
npm install --save-dev @playwright/test @playwright/mcp @playwright/cli dotenv @faker-js/faker allure-playwright allure-commandline prettier

# 3. Instalar browsers de Playwright
npx playwright install --with-deps

# 4. Instalar skills del Playwright CLI
playwright-cli install --skills
```

**Estructura a crear según tipo:**

#### Tipo: `hybrid` (Web + API)
```bash
# Crear estructura de carpetas
mkdir -p tests/ui/auth tests/ui/navigation tests/ui/forms
mkdir -p tests/api/contracts tests/api/integration tests/api/performance
mkdir -p tests/hybrid
mkdir -p fixtures utils data/schemas reports/allure-results reports/allure-report
```

#### Tipo: `api` (Solo API)
```bash
mkdir -p tests/api/contracts tests/api/integration tests/api/performance
mkdir -p fixtures utils data/schemas reports/allure-results reports/allure-report
```

#### Tipo: `web` (Solo Web)
```bash
mkdir -p tests/ui/auth tests/ui/navigation tests/ui/forms
mkdir -p fixtures utils data reports/allure-results reports/allure-report
```

---

### `configurePlaywright(type)`
Genera el archivo `playwright.config.js` adaptado al tipo de proyecto.

#### Config para Hybrid (Web + API)
```javascript
// playwright.config.js
// @ts-check
import { defineConfig, devices } from '@playwright/test';
import dotenv from 'dotenv';
import path from 'path';

dotenv.config({ path: path.resolve(process.cwd(), '.env') });

export default defineConfig({
  testDir: './tests',
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: [
    ['html'],
    ['allure-playwright'],
  ],
  use: {
    baseURL: process.env.BASE_URL || 'http://localhost:3000',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
  },
  projects: [
    {
      name: 'ui',
      testDir: './tests/ui',
      use: { ...devices['Desktop Chrome'] },
    },
    {
      name: 'api',
      testDir: './tests/api',
      use: {
        baseURL: process.env.API_BASE_URL || 'http://localhost:3000/api',
        extraHTTPHeaders: {
          'Accept': 'application/json',
          'Content-Type': 'application/json',
        },
      },
    },
    {
      name: 'hybrid',
      testDir: './tests/hybrid',
      use: { ...devices['Desktop Chrome'] },
    },
  ],
});
```

#### Config para API Pura
```javascript
// playwright.config.js
// @ts-check
import { defineConfig } from '@playwright/test';
import dotenv from 'dotenv';
import path from 'path';

dotenv.config({ path: path.resolve(process.cwd(), '.env') });

export default defineConfig({
  testDir: './tests/api',
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 2 : undefined,
  reporter: [
    ['html'],
    ['allure-playwright'],
  ],
  use: {
    baseURL: process.env.API_BASE_URL || 'http://localhost:3000/api',
    extraHTTPHeaders: {
      'Accept': 'application/json',
      'Content-Type': 'application/json',
    },
  },
});
```

---

### `setupMCP()`
Configura el servidor MCP de Playwright para VS Code.

**Archivo: `.vscode/mcp.json`**
```json
{
  "inputs": [
    {
      "id": "playwright-browser",
      "type": "pickString",
      "description": "Browser to use for Playwright MCP",
      "default": "chromium",
      "options": ["chromium", "firefox", "webkit", "chrome", "msedge"]
    },
    {
      "id": "playwright-headless",
      "type": "pickString",
      "description": "Run browser in headless mode?",
      "default": "true",
      "options": ["true", "false"]
    }
  ],
  "servers": {
    "playwright": {
      "command": "npx",
      "args": [
        "@playwright/mcp@latest",
        "--browser=${input:playwright-browser}",
        "--headless=${input:playwright-headless}",
        "--caps=testing,vision,pdf"
      ]
    }
  }
}
```

---

### `createAPITestStructure()`
Crea archivos base para testing de API.

#### Fixture de API Client: `fixtures/api.fixture.js`
```javascript
import { test as base } from '@playwright/test';

/**
 * Fixture que proporciona un API client autenticado
 */
export const test = base.extend({
  apiClient: async ({ playwright }, use) => {
    const context = await playwright.request.newContext({
      baseURL: process.env.API_BASE_URL || 'http://localhost:3000/api',
      extraHTTPHeaders: {
        'Authorization': `Bearer ${process.env.API_TOKEN}`,
        'Accept': 'application/json',
        'Content-Type': 'application/json',
      },
    });
    await use(context);
    await context.dispose();
  },
});

export { expect } from '@playwright/test';
```

#### Helper de Datos: `utils/data-generator.js`
```javascript
import { faker } from '@faker-js/faker';

export const DataGenerator = {
  user: () => ({
    name: faker.person.fullName(),
    email: faker.internet.email(),
    password: faker.internet.password({ length: 12 }),
    phone: faker.phone.number(),
  }),

  address: () => ({
    street: faker.location.streetAddress(),
    city: faker.location.city(),
    state: faker.location.state(),
    zip: faker.location.zipCode(),
    country: faker.location.country(),
  }),

  product: () => ({
    name: faker.commerce.productName(),
    price: parseFloat(faker.commerce.price()),
    description: faker.commerce.productDescription(),
    category: faker.commerce.department(),
    sku: faker.string.alphanumeric(10).toUpperCase(),
  }),

  /** Genera N items usando un generador dado */
  batch: (generator, count = 5) =>
    Array.from({ length: count }, () => generator()),
};
```

---

### `createUITestStructure()`
Crea archivos base para testing de UI.

#### Fixture de Auth: `fixtures/auth.fixture.js`
```javascript
import { test as base } from '@playwright/test';

/**
 * Fixture que proporciona una página autenticada
 */
export const test = base.extend({
  authenticatedPage: async ({ page }, use) => {
    // Login
    await page.goto('/login');
    await page.getByLabel('Email').fill(process.env.AUTH_USERNAME || 'test@example.com');
    await page.getByLabel('Password').fill(process.env.AUTH_PASSWORD || 'password');
    await page.getByRole('button', { name: /sign in|log in/i }).click();
    await page.waitForURL('**/dashboard**');

    await use(page);
  },
});

export { expect } from '@playwright/test';
```

---

### `installDependencies()`
Instala todas las dependencias del proyecto.

```bash
# Instalar dependencias npm
npm install

# Instalar browsers de Playwright
npx playwright install --with-deps

# Instalar skills del CLI
playwright-cli install --skills

# Verificar instalación
npx playwright --version
playwright-cli --version
npm ls --depth=0
```

---

## Archivos de Configuración Adicionales

### `.env.example`
```bash
# ═══════════════════════════════════════════
# QA Automation — Variables de Entorno
# ═══════════════════════════════════════════

# Base URLs
BASE_URL=http://localhost:3000
API_BASE_URL=http://localhost:3000/api

# Authentication
AUTH_USERNAME=test-user@example.com
AUTH_PASSWORD=test-password
API_TOKEN=your-api-token-here

# Environment
NODE_ENV=test
CI=false

# Playwright
PLAYWRIGHT_BROWSERS_PATH=0
```

### `jsconfig.json`
```json
{
  "compilerOptions": {
    "module": "ESNext",
    "moduleResolution": "bundler",
    "target": "ESNext",
    "checkJs": false,
    "resolveJsonModule": true,
    "baseUrl": ".",
    "paths": {
      "@fixtures/*": ["fixtures/*"],
      "@utils/*": ["utils/*"],
      "@data/*": ["data/*"]
    }
  },
  "include": ["tests/**/*.js", "fixtures/**/*.js", "utils/**/*.js"],
  "exclude": ["node_modules"]
}
```

### `.prettierrc`
```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "all",
  "printWidth": 100,
  "bracketSpacing": true,
  "arrowParens": "always"
}
```

### `.gitignore`
```
node_modules/
test-results/
playwright-report/
allure-results/
allure-report/
blob-report/
.env
*.auth.json
```

---

## Comandos de Verificación

```bash
# Dry-run: ejecutar un test vacío para verificar configuración
npx playwright test --list

# Verificar que la config es válida
npx playwright test --config=playwright.config.js --list

# Verificar formato de código
npx prettier --check "**/*.{js,json,md}"

# Formatear código
npx prettier --write "**/*.{js,json,md}"
```
