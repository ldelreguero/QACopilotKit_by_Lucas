# SKILL: QA Analyst

> Funciones y comandos para análisis de datos, JSON Diffing, reportes de regresión y validación de integridad.

---

## Funciones de Análisis

### `compareJSON(baseline, current)`
Compara dos objetos/archivos JSON y produce un reporte de diferencias.

```javascript
// utils/json-diff.js

/**
 * Deep diff entre dos objetos JSON
 * @param {Object} baseline - Objeto esperado (anterior / golden)
 * @param {Object} current - Objeto actual (nuevo)
 * @param {string} [path=''] - Ruta acumulada (para recursión)
 * @returns {Array<DiffEntry>} Lista de diferencias
 *
 * @typedef {Object} DiffEntry
 * @property {'ADDED'|'REMOVED'|'CHANGED'|'TYPE_CHANGED'} type
 * @property {string} path
 * @property {*} [oldValue]
 * @property {*} [newValue]
 * @property {'CRITICAL'|'HIGH'|'MEDIUM'|'LOW'} severity
 */
export function deepDiff(baseline, current, path = '') {
  const diffs = [];

  if (baseline === null || current === null || typeof baseline !== 'object' || typeof current !== 'object') {
    if (baseline !== current) {
      diffs.push({
        type: typeof baseline !== typeof current ? 'TYPE_CHANGED' : 'CHANGED',
        path: path || '(root)',
        oldValue: baseline,
        newValue: current,
        severity: typeof baseline !== typeof current ? 'CRITICAL' : 'MEDIUM',
      });
    }
    return diffs;
  }

  const isArrayBaseline = Array.isArray(baseline);
  const isArrayCurrent = Array.isArray(current);

  if (isArrayBaseline !== isArrayCurrent) {
    diffs.push({
      type: 'TYPE_CHANGED',
      path: path || '(root)',
      oldValue: isArrayBaseline ? 'Array' : 'Object',
      newValue: isArrayCurrent ? 'Array' : 'Object',
      severity: 'CRITICAL',
    });
    return diffs;
  }

  // Detectar campos eliminados
  for (const key of Object.keys(baseline)) {
    const fullPath = path ? `${path}.${key}` : key;
    if (!(key in current)) {
      diffs.push({
        type: 'REMOVED',
        path: fullPath,
        oldValue: baseline[key],
        severity: 'HIGH',
      });
    }
  }

  // Detectar campos nuevos y cambios
  for (const key of Object.keys(current)) {
    const fullPath = path ? `${path}.${key}` : key;
    if (!(key in baseline)) {
      diffs.push({
        type: 'ADDED',
        path: fullPath,
        newValue: current[key],
        severity: 'MEDIUM',
      });
    } else if (typeof baseline[key] === 'object' && typeof current[key] === 'object') {
      if (baseline[key] !== null && current[key] !== null) {
        diffs.push(...deepDiff(baseline[key], current[key], fullPath));
      } else if (baseline[key] !== current[key]) {
        diffs.push({
          type: 'CHANGED',
          path: fullPath,
          oldValue: baseline[key],
          newValue: current[key],
          severity: 'HIGH',
        });
      }
    } else if (baseline[key] !== current[key]) {
      diffs.push({
        type: typeof baseline[key] !== typeof current[key] ? 'TYPE_CHANGED' : 'CHANGED',
        path: fullPath,
        oldValue: baseline[key],
        newValue: current[key],
        severity: typeof baseline[key] !== typeof current[key] ? 'CRITICAL' : 'MEDIUM',
      });
    }
  }

  return diffs;
}

/**
 * Formatea las diferencias como tabla Markdown
 * @param {Array<DiffEntry>} diffs
 * @returns {string} Tabla Markdown
 */
export function formatDiffAsMarkdown(diffs) {
  if (diffs.length === 0) return '✅ **Sin diferencias detectadas.**\n';

  const lines = [
    `| Severidad | Tipo | Path | Valor Anterior | Valor Actual |`,
    `|-----------|------|------|---------------|--------------|`,
  ];

  const sorted = [...diffs].sort((a, b) => {
    const order = { CRITICAL: 0, HIGH: 1, MEDIUM: 2, LOW: 3 };
    return (order[a.severity] ?? 4) - (order[b.severity] ?? 4);
  });

  for (const d of sorted) {
    const icon = { CRITICAL: '🔴', HIGH: '🟠', MEDIUM: '🟡', LOW: '🟢' }[d.severity];
    const old = d.oldValue !== undefined ? `\`${JSON.stringify(d.oldValue)}\`` : '—';
    const now = d.newValue !== undefined ? `\`${JSON.stringify(d.newValue)}\`` : '—';
    lines.push(`| ${icon} ${d.severity} | ${d.type} | \`${d.path}\` | ${old} | ${now} |`);
  }

  return lines.join('\n');
}
```

---

### `validateContract(response, schema)`
Valida una response de API contra un JSON Schema.

```javascript
// utils/contract-validator.js

/**
 * Valida un response body contra un JSON Schema simplificado
 * @param {Object} response - Response body
 * @param {Object} schema - Schema con { required, properties, additionalProperties }
 * @returns {ValidationResult}
 *
 * @typedef {Object} ValidationResult
 * @property {boolean} valid
 * @property {Array<Violation>} violations
 * @property {string} checkedAt
 *
 * @typedef {Object} Violation
 * @property {'MISSING_REQUIRED'|'TYPE_MISMATCH'|'UNEXPECTED_FIELD'|'ENUM_VIOLATION'|'RANGE_VIOLATION'} type
 * @property {string} field
 * @property {'CRITICAL'|'HIGH'|'MEDIUM'|'LOW'} severity
 * @property {string} message
 */
export function validateContract(response, schema) {
  const violations = [];

  // 1. Campos requeridos
  if (schema.required) {
    for (const field of schema.required) {
      if (response === null || response === undefined || !(field in response)) {
        violations.push({
          type: 'MISSING_REQUIRED',
          field,
          severity: 'CRITICAL',
          message: `Campo requerido '${field}' ausente en la response`,
        });
      }
    }
  }

  // 2. Tipos de datos
  if (schema.properties && response) {
    for (const [field, spec] of Object.entries(schema.properties)) {
      if (field in response) {
        const value = response[field];
        const actualType = value === null ? 'null' : Array.isArray(value) ? 'array' : typeof value;

        // Verificar tipo
        if (spec.type && actualType !== spec.type) {
          violations.push({
            type: 'TYPE_MISMATCH',
            field,
            severity: 'HIGH',
            message: `'${field}': esperado ${spec.type}, recibido ${actualType}`,
            expected: spec.type,
            actual: actualType,
          });
        }

        // Verificar enum
        if (spec.enum && !spec.enum.includes(value)) {
          violations.push({
            type: 'ENUM_VIOLATION',
            field,
            severity: 'MEDIUM',
            message: `'${field}': valor '${value}' no está en enum [${spec.enum.join(', ')}]`,
          });
        }

        // Verificar rango numérico
        if (spec.type === 'number' && typeof value === 'number') {
          if (spec.minimum !== undefined && value < spec.minimum) {
            violations.push({
              type: 'RANGE_VIOLATION',
              field,
              severity: 'MEDIUM',
              message: `'${field}': valor ${value} menor que mínimo ${spec.minimum}`,
            });
          }
          if (spec.maximum !== undefined && value > spec.maximum) {
            violations.push({
              type: 'RANGE_VIOLATION',
              field,
              severity: 'MEDIUM',
              message: `'${field}': valor ${value} mayor que máximo ${spec.maximum}`,
            });
          }
        }
      }
    }
  }

  // 3. Campos no esperados
  if (schema.additionalProperties === false && schema.properties && response) {
    const expectedFields = new Set(Object.keys(schema.properties));
    for (const field of Object.keys(response)) {
      if (!expectedFields.has(field)) {
        violations.push({
          type: 'UNEXPECTED_FIELD',
          field,
          severity: 'LOW',
          message: `Campo inesperado '${field}' encontrado en la response`,
        });
      }
    }
  }

  return {
    valid: violations.length === 0,
    violations,
    summary: {
      total: violations.length,
      critical: violations.filter((v) => v.severity === 'CRITICAL').length,
      high: violations.filter((v) => v.severity === 'HIGH').length,
      medium: violations.filter((v) => v.severity === 'MEDIUM').length,
      low: violations.filter((v) => v.severity === 'LOW').length,
    },
    checkedAt: new Date().toISOString(),
  };
}
```

---

### `analyzeNetworkLogs(requests)`
Analiza tráfico de red capturado con Playwright MCP.

```javascript
// utils/network-analyzer.js

/**
 * Analiza un array de network requests (capturados vía browser_network_requests)
 * @param {Array<NetworkRequest>} requests
 * @returns {NetworkAnalysis}
 */
export function analyzeNetworkLogs(requests) {
  const analysis = {
    total: requests.length,
    timestamp: new Date().toISOString(),
    byStatus: {},
    byMethod: {},
    byType: {},
    failedRequests: [],
    slowRequests: [],
    largeResponses: [],
    stats: {
      avgDuration: 0,
      maxDuration: 0,
      errorRate: 0,
    },
  };

  let totalDuration = 0;
  let errorCount = 0;

  for (const req of requests) {
    // Agrupar por status code
    const statusGroup = req.status ? `${Math.floor(req.status / 100)}xx` : 'pending';
    analysis.byStatus[statusGroup] = (analysis.byStatus[statusGroup] || 0) + 1;

    // Agrupar por método HTTP
    const method = (req.method || 'GET').toUpperCase();
    analysis.byMethod[method] = (analysis.byMethod[method] || 0) + 1;

    // Agrupar por tipo de recurso
    const type = req.resourceType || 'other';
    analysis.byType[type] = (analysis.byType[type] || 0) + 1;

    // Tracking de duración
    if (req.duration) {
      totalDuration += req.duration;
      if (req.duration > analysis.stats.maxDuration) {
        analysis.stats.maxDuration = req.duration;
      }
    }

    // Detectar errores (4xx, 5xx)
    if (req.status >= 400) {
      errorCount++;
      analysis.failedRequests.push({
        url: req.url,
        method,
        status: req.status,
        statusText: req.statusText || '',
        severity: req.status >= 500 ? 'CRITICAL' : 'HIGH',
      });
    }

    // Detectar requests lentos (> 3s)
    if (req.duration && req.duration > 3000) {
      analysis.slowRequests.push({
        url: req.url,
        method,
        duration: `${(req.duration / 1000).toFixed(2)}s`,
        severity: req.duration > 10000 ? 'HIGH' : 'MEDIUM',
      });
    }

    // Detectar responses grandes (> 1MB)
    if (req.responseSize && req.responseSize > 1048576) {
      analysis.largeResponses.push({
        url: req.url,
        size: `${(req.responseSize / 1048576).toFixed(2)} MB`,
        severity: req.responseSize > 5242880 ? 'HIGH' : 'MEDIUM',
      });
    }
  }

  analysis.stats.avgDuration = requests.length > 0 ? Math.round(totalDuration / requests.length) : 0;
  analysis.stats.errorRate = requests.length > 0 ? ((errorCount / requests.length) * 100).toFixed(1) + '%' : '0%';

  return analysis;
}
```

---

### `regressionCheck(currentResults, baselineResults)`
Compara resultados de ejecución actuales contra un baseline para detectar regresiones.

```javascript
// utils/regression-checker.js

/**
 * Detecta regresiones comparando resultados de test actuales vs baseline
 * @param {Array<TestResult>} currentResults
 * @param {Array<TestResult>} baselineResults
 * @returns {RegressionReport}
 */
export function regressionCheck(currentResults, baselineResults) {
  const report = {
    timestamp: new Date().toISOString(),
    newFailures: [],       // Tests que antes pasaban y ahora fallan
    fixedTests: [],        // Tests que antes fallaban y ahora pasan
    consistentFailures: [], // Tests que siguen fallando
    newTests: [],          // Tests que no existían en el baseline
    removedTests: [],      // Tests que ya no existen
    summary: {},
  };

  const baselineMap = new Map(baselineResults.map((t) => [t.name, t]));
  const currentMap = new Map(currentResults.map((t) => [t.name, t]));

  // Analizar tests actuales contra baseline
  for (const test of currentResults) {
    const baseline = baselineMap.get(test.name);

    if (!baseline) {
      report.newTests.push({ name: test.name, status: test.status });
      continue;
    }

    if (baseline.status === 'passed' && test.status === 'failed') {
      report.newFailures.push({
        name: test.name,
        previousStatus: 'passed',
        currentStatus: 'failed',
        error: test.error || 'Unknown error',
        severity: 'CRITICAL',
      });
    } else if (baseline.status === 'failed' && test.status === 'passed') {
      report.fixedTests.push({
        name: test.name,
        previousStatus: 'failed',
        currentStatus: 'passed',
      });
    } else if (baseline.status === 'failed' && test.status === 'failed') {
      report.consistentFailures.push({
        name: test.name,
        error: test.error || 'Unknown error',
      });
    }
  }

  // Detectar tests eliminados
  for (const test of baselineResults) {
    if (!currentMap.has(test.name)) {
      report.removedTests.push({ name: test.name, previousStatus: test.status });
    }
  }

  report.summary = {
    totalCurrent: currentResults.length,
    totalBaseline: baselineResults.length,
    regressions: report.newFailures.length,
    fixes: report.fixedTests.length,
    persistentFailures: report.consistentFailures.length,
    newTests: report.newTests.length,
    removedTests: report.removedTests.length,
    healthScore: currentResults.length > 0
      ? ((currentResults.filter((t) => t.status === 'passed').length / currentResults.length) * 100).toFixed(1) + '%'
      : '0%',
  };

  return report;
}
```

---

### `generateAllureReport()`
Genera y gestiona reportes con Allure.

```bash
# ═══════════════════════════════════════════
# ALLURE — Generación de Reportes
# ═══════════════════════════════════════════

# Generar reporte HTML de Allure desde los resultados
npx allure generate allure-results --clean -o allure-report

# Abrir reporte Allure en el navegador
npx allure open allure-report

# Generar y abrir en un solo paso
npx allure generate allure-results --clean -o allure-report && npx allure open allure-report

# Ver historial de ejecuciones
# (Copiar allure-report/history/ a allure-results/history/ antes de generar)
cp -r allure-report/history allure-results/history 2>/dev/null; npx allure generate allure-results --clean -o allure-report

# ═══════════════════════════════════════════
# EJECUCIÓN CON ALLURE REPORTER
# ═══════════════════════════════════════════

# Ejecutar tests con reporter Allure
npx playwright test --reporter=allure-playwright

# Ejecutar con múltiples reporters
npx playwright test --reporter=html,allure-playwright

# Ejecutar tests de API con Allure
npx playwright test --project=api --reporter=allure-playwright
```

---

### `snapshotCompare(beforeSnapshot, afterSnapshot)`
Compara dos accessibility snapshots capturados con MCP.

```javascript
// utils/snapshot-compare.js

/**
 * Compara dos accessibility snapshots (obtenidos via browser_snapshot)
 * para detectar cambios en la estructura de la página
 * @param {Object} before - Snapshot anterior
 * @param {Object} after - Snapshot actual
 * @returns {SnapshotDiff}
 */
export function snapshotCompare(before, after) {
  const diff = {
    addedElements: [],
    removedElements: [],
    changedElements: [],
    structuralChanges: false,
  };

  const beforeElements = flattenSnapshot(before);
  const afterElements = flattenSnapshot(after);

  const beforeMap = new Map(beforeElements.map((e) => [elementKey(e), e]));
  const afterMap = new Map(afterElements.map((e) => [elementKey(e), e]));

  // Elementos añadidos
  for (const [key, element] of afterMap) {
    if (!beforeMap.has(key)) {
      diff.addedElements.push(element);
    }
  }

  // Elementos eliminados
  for (const [key, element] of beforeMap) {
    if (!afterMap.has(key)) {
      diff.removedElements.push(element);
    }
  }

  // Elementos con cambios
  for (const [key, afterEl] of afterMap) {
    const beforeEl = beforeMap.get(key);
    if (beforeEl && JSON.stringify(beforeEl) !== JSON.stringify(afterEl)) {
      diff.changedElements.push({ before: beforeEl, after: afterEl });
    }
  }

  diff.structuralChanges =
    diff.addedElements.length > 0 ||
    diff.removedElements.length > 0 ||
    diff.changedElements.length > 0;

  return diff;
}

function flattenSnapshot(snapshot, result = []) {
  if (!snapshot) return result;
  result.push({ role: snapshot.role, name: snapshot.name, value: snapshot.value });
  if (snapshot.children) {
    for (const child of snapshot.children) {
      flattenSnapshot(child, result);
    }
  }
  return result;
}

function elementKey(element) {
  return `${element.role}::${element.name}`;
}
```

---

## Plantillas de Reportes

### Reporte de Regresión
```markdown
# 📊 Reporte de Regresión — {{FECHA}}

## Resumen Ejecutivo

| Métrica | Valor |
|---------|-------|
| Tests ejecutados | {{TOTAL}} |
| Pasados | {{PASSED}} ✅ |
| Fallidos | {{FAILED}} ❌ |
| **Regresiones nuevas** | {{REGRESSIONS}} 🔴 |
| Tests arreglados | {{FIXED}} 🟢 |
| Tests nuevos | {{NEW}} 🔵 |
| Health Score | {{SCORE}}% |

## 🔴 Regresiones Detectadas

| # | Test | Error | Severidad |
|---|------|-------|-----------|
{{#EACH regression}}
| {{INDEX}} | `{{NAME}}` | {{ERROR}} | {{SEVERITY}} |
{{/EACH}}

## 🟢 Tests Arreglados

{{#EACH fix}}
- ✅ `{{NAME}}` — Antes fallaba, ahora pasa
{{/EACH}}

## Recomendaciones

1. {{RECOMENDACIÓN_1}}
2. {{RECOMENDACIÓN_2}}
3. {{RECOMENDACIÓN_3}}
```

### Reporte de Integridad de API
```markdown
# 🔍 Reporte de Integridad — {{FECHA}}

## Contratos Validados

| Endpoint | Schema | ¿Válido? | Violaciones |
|----------|--------|----------|-------------|
{{#EACH contract}}
| `{{METHOD}} {{PATH}}` | `{{SCHEMA}}` | {{STATUS}} | {{VIOLATIONS}} |
{{/EACH}}

## Violaciones Detalladas

{{#EACH violation}}
### {{SEVERITY}} — `{{FIELD}}` en `{{ENDPOINT}}`
- **Tipo**: {{TYPE}}
- **Esperado**: {{EXPECTED}}
- **Actual**: {{ACTUAL}}
- **Mensaje**: {{MESSAGE}}
{{/EACH}}

## Network Analysis

| Métrica | Valor |
|---------|-------|
| Total requests | {{TOTAL}} |
| Error rate | {{ERROR_RATE}} |
| Avg duration | {{AVG_DURATION}} |
| Slow requests (>3s) | {{SLOW}} |
```

---

## Uso de MCP para Análisis

### Capturar tráfico de red
```
1. browser_navigate → URL del flujo a analizar
2. Ejecutar acciones del flujo (clicks, formularios, etc.)
3. browser_network_requests → Obtener todas las peticiones capturadas
4. analyzeNetworkLogs() → Procesar y clasificar el tráfico
5. Generar reporte con hallazgos
```

### Comparar UI vs API
```
1. browser_navigate → Página que muestra datos
2. browser_snapshot → Capturar datos visibles en la UI
3. fetch (o request) → Obtener los mismos datos vía API
4. deepDiff() → Comparar lo que muestra la UI vs lo que retorna la API
5. Reportar discrepancias si las hay
```
