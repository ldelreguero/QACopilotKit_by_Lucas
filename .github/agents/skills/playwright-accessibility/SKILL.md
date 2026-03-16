---
name: playwright-accessibility
description: Define y ejecuta pruebas de accesibilidad con Playwright, axe-core y verificacion manual de teclado, foco y semantica. Usar cuando el usuario pida crear, revisar o depurar cobertura WCAG.
license: Proprietary
compatibility: Diseñada para GitHub Copilot en VS Code con Playwright; puede apoyarse en axe-core y MCP si el proyecto ya los usa.
metadata:
  author: Lucas del Reguero Martinez
  version: "1.0"
---

# Playwright accessibility

## Cuando usar

Usa esta skill cuando la tarea implique:

- crear pruebas de accesibilidad para UI web
- combinar escaneo automatico con verificacion manual
- revisar navegacion por teclado, foco, labels o landmarks
- validar dialogos, formularios o componentes interactivos bajo criterios WCAG

## Resultado esperado

La salida debe dejar:

1. cobertura automatica minima definida
2. verificacion manual critica identificada
3. criterios WCAG o heuristicas equivalentes explicitados
4. evidencia de ejecucion o limitacion indicada

## Instrucciones

1. Empieza por el riesgo real.
   Prioriza formularios, modales, navegacion, tablas, errores y componentes custom antes que paginas estaticas.
2. Combina automatizacion y validacion manual.
   Usa axe-core para violaciones estructurales y completa con teclado, orden de tab, foco visible y nombres accesibles.
3. Favorece HTML nativo.
   Si aparece ARIA innecesario o contradictorio, tratalo como deuda de accesibilidad y no como solucion.
4. Verifica foco y operabilidad.
   Todo elemento interactivo debe ser alcanzable, accionable y cerrable con teclado segun el patron del componente.
5. Valida semantica y percepcion.
   Revisa headings, landmarks, labels, texto alternativo, mensajes de error y contenido oculto para lectores.
6. Reporta con impacto.
   Separa violaciones criticas de mejoras menores y explica el riesgo funcional o legal.

## Cobertura minima sugerida

- escaneo con axe-core en las vistas criticas
- prueba de tabulacion sobre el flujo principal
- validacion de foco inicial, trampa y retorno en modales
- chequeo de labels, descripciones y mensajes de error en formularios
- verificacion de roles, nombres accesibles y estado expandido/seleccionado en widgets custom

## Recursos relacionados

- [WCAG y Playwright](references/wcag-y-playwright.md)
- [Playwright quality gate](../playwright-best-practices/references/quality-gate.md)
- [Accessibility base](../playwright-best-practices/references/accessibility.md)
- [qa-ejecutor](../qa-ejecutor/SKILL.md)
- [playwright-best-practices](../playwright-best-practices/SKILL.md)

## Casos borde

- Si el proyecto no tiene axe-core, no lo agregues sin justificar dependencia y pipeline.
- Si una prueba visual detecta un problema de contraste, confirma si es una falla real de accesibilidad o una limitacion del baseline.
- Si no existe soporte semantico suficiente en la UI, recomienda cambios de producto antes de endurecer el test.