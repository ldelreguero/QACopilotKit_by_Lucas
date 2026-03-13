# Configuración y parametrización

## Objetivo

Concentrar criterios mínimos para configurar proyectos QA y parametrizar suites sin abrir una skill separada para problemas que ya pertenecen al setup base.

## Configuración

- mantener una configuración mínima y legible antes de añadir capas
- separar variables de entorno, projects, reporters y timeouts por responsabilidad
- justificar cada dependencia y cada reporter
- evitar configuraciones mágicas que oculten estado o credenciales

## Parametrización

- parametrizar cuando reduce duplicación real, no por estilo
- fijar claramente qué dimensión cambia: navegador, rol, dataset, locale o endpoint
- preferir datasets pequeños y representativos antes de combinaciones explosivas
- mantener trazabilidad entre caso parametrizado y fallo observado

## Cuándo integrarlo en el diseño

- al definir `projects`, matrices de entornos o variantes de ejecución
- al modelar datasets de API, roles o combinaciones de smoke
- al decidir si un caso merece parametrización o una suite separada

## Regla práctica

Si una necesidad de configuración o parametrización puede resolverse dentro del setup base del repo, debe vivir en `qa-arquitecto` y no abrir un dominio nuevo.

## Salida esperada

Decisión clara sobre configuración mínima, variables, matrices y parametrización necesaria para mantener la suite estable y mantenible.
