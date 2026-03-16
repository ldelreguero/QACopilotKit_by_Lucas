---
name: playwright-visual-testing
description: Diseña y ejecuta pruebas visuales con Playwright basadas en screenshots, baselines y diffs controlados. Usar cuando el usuario pida visual regression, screenshot diff o cobertura responsive en CI.
license: Proprietary
compatibility: Diseñada para GitHub Copilot en VS Code con Playwright y pipeline capaz de almacenar baselines o artefactos visuales.
metadata:
  author: Lucas del Reguero Martinez
  version: "1.0"
---

# Playwright visual testing

## Cuando usar

Usa esta skill cuando la tarea implique:

- crear pruebas de visual regression
- comparar screenshots contra un baseline
- estabilizar diffs visuales ruidosos
- cubrir breakpoints o estados de UI con evidencia visual

## Resultado esperado

La salida debe dejar:

1. estrategia de baseline definida
2. fuentes de ruido visual controladas
3. aserciones visuales con umbral justificado
4. criterio claro para actualizar snapshots

## Instrucciones

1. Haz deterministico el render.
   Controla animaciones, loaders, fechas, datos variables, fuentes y anuncios antes de capturar.
2. Define el nivel correcto.
   Usa screenshot de pagina completa para layout y de componente para estados concretos.
3. Enmascara lo que no sea estable.
   No normalices todo; enmascara solo contadores, avatares, clocks, charts o terceros inevitables.
4. Cubre viewport y estado.
   Prioriza desktop y mobile criticos, junto con hover, error, vacio y success donde el diseño cambie.
5. Trata el diff como señal, no como binario.
   Explica si el cambio es esperado, cosmetico, regresion o ruido de entorno.
6. Asegura mantenimiento.
   Documenta cuando corresponde regenerar baselines y quien debe aprobarlos.

## Cobertura minima sugerida

- una prueba de pagina critica
- una prueba de componente sensible al diseño
- un breakpoint movil o tablet relevante
- una regla de enmascaramiento para contenido dinamico
- artefactos o reporter que permitan revisar el diff

## Recursos relacionados

- [Baselines y diffs](references/baselines-y-diffs.md)
- [Playwright Skill](../playwright-skill/SKILL.md)
- [Visual organization base](../playwright-skill/core/test-organization.md)
- [qa-ejecutor](../qa-ejecutor/SKILL.md)
- [playwright-skill](../playwright-skill/SKILL.md)

## Casos borde

- Si el entorno no garantiza fuentes o viewport consistentes, baja alcance antes de introducir flakes visuales.
- Si el proyecto no quiere snapshots en git, define almacenamiento de artefactos en CI antes de crear la suite.
- Si el producto cambia con frecuencia sin disciplina de diseño, limita la cobertura visual a paginas o componentes de alto impacto.