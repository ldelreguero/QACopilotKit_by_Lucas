---
name: api-suite-generation
description: Diseña suites API a partir de endpoints REST u OpenAPI, cubriendo CRUD, negativos, autenticacion y datos. Usar cuando el usuario necesite planificar o scaffoldear cobertura API automatizada.

compatibility: Diseñada para GitHub Copilot en VS Code con proyectos Node.js y runners de testing ya definidos o por definir.

---

# API suite generation

## Cuando usar

Usa esta skill cuando la tarea implique:

- generar una suite API desde cero
- traducir OpenAPI o endpoints documentados a casos automatizados
- definir cobertura CRUD, auth, paginacion y errores
- proponer estructura, fixtures y datos para pruebas API

## Resultado esperado

La salida debe dejar:

1. estructura de suite y carpetas definida
2. endpoints priorizados por riesgo
3. matriz minima de casos positivos y negativos
4. estrategia de datos y limpieza acordada

## Instrucciones

1. Parte del contrato o inventario de endpoints.
   Si existe OpenAPI, usalo como fuente; si no, deriva un inventario explicito antes de generar tests.
2. Cubre por recurso.
   Agrupa por dominio funcional y evita suites gigantes con endpoints mezclados.
3. Genera negativos desde el diseño.
   Para cada happy path incluye auth ausente, payload invalido y conflicto o not found relevante.
4. Diseña datos reutilizables.
   Define fixtures, factories y cleanup antes de escalar la cantidad de casos.
5. Separa smoke de profundidad.
   Deja un subset rapido para CI temprano y mueve combinatoria pesada a capas posteriores.
6. Mantén independencia.
   Ningun test debe depender del orden de ejecucion ni de datos persistidos por otro.

## Recursos relacionados

- [Diseño de suite API](references/diseno-de-suite-api.md)
- [qa-arquitecto](../qa-arquitecto/SKILL.md)
- [playwright-api](../playwright-api/SKILL.md)

## Casos borde

- Si el spec esta incompleto, declara huecos antes de autogenerar cobertura.
- Si el entorno no permite cleanup, diseña datos unicos y tolerantes a reruns.
- Si hay multiples consumidores, prioriza endpoints criticos y no cantidad bruta de tests.