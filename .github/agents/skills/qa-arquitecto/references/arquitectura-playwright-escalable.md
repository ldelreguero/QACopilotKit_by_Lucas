# Arquitectura Playwright escalable

Esta guía resume el delta útil extraído de las qaskills legacy de arquitectura Playwright sin copiar su contenido completo.

## Cuándo conviene una arquitectura avanzada

Usa una arquitectura más profunda cuando se cumplan varias de estas condiciones:

- más de una suite importante: UI, API, visual o integración
- varios equipos o contribuyentes tocando tests al mismo tiempo
- necesidad de reutilizar flujos complejos de negocio
- múltiples browsers, devices o entornos
- necesidad de reporting, trazabilidad y CI más estrictos

Si el proyecto todavía es pequeño, empieza con una estructura mínima y escala después.

## Regla práctica de evolución

1. `MVP`: `tests/`, `fixtures/`, `utils/`, `data/`, `playwright.config.*`
2. `Intermedio`: sumar `pages/`, `tests/api/`, `tests/ui/`, `tests/hybrid/`
3. `Avanzado`: separar `pages/`, `modules/`, `api/`, `fixtures/`, `config/`, `testdata/`, reporters y CI

## Patrón recomendado para proyectos grandes

### Capas útiles

1. Configuración: variables, projects, retries, timeouts, reporters.
2. Pages: locators y acciones UI básicas.
3. Modules: flujos de negocio y orquestación entre páginas.
4. Utils: factories, waits reutilizables, logging y helpers puros.
5. API: clientes y helpers para setup o validaciones fuera de UI.
6. Fixtures: inyección de dependencias y estado autenticado.
7. Reporting: adjuntos, trazas, categorización y salida de resultados.
8. CI/CD: paralelismo, sharding, publicación de reportes y quality gates.

## Regla de separación de responsabilidades

- `Pages`: locators y acciones simples; evitar lógica de negocio compleja.
- `Modules`: flujos multi-step, condicionales y reglas de negocio del test.
- `Tests`: intención, assertions y composición de módulos o fixtures.

## Cuándo usar POM simple vs pages + modules

- POM simple: suites pequeñas o medianas con flujos directos.
- Pages + Modules: journeys largos, checkout complejos, alta reutilización o muchos condicionales.

## Decisiones que conviene fijar temprano

- naming consistente de suites, pages, fixtures y test data
- estrategia de autenticación reutilizable
- dónde viven contratos, mocks y factories
- reporter base y artefactos mínimos de fallo
- política de retries y paralelismo por entorno

## Señales de que el framework debe refactorizarse

- pages con demasiada lógica y assertions mezcladas
- tests largos con pasos repetidos entre suites
- fixtures que ocultan demasiada magia y vuelven opaco el flujo
- estructura distinta en cada feature
- CI lenta o inestable sin una estrategia clara de sharding o aislamiento

## Referencias relacionadas

- [Blueprints de setup](blueprints-de-setup.md)
- [Playwright Skill](../../playwright-skill/SKILL.md)
- [Page Object Model](../../playwright-skill/pom/page-object-model.md)
- [Test Organization](../../playwright-skill/core/test-organization.md)
- [Fixtures & Hooks](../../playwright-skill/core/fixtures-and-hooks.md)
- [Organización de tests y suites](../../playwright-skill/core/test-organization.md)
