---
description: "Inicia desde cero un proyecto de automatizacion API con Playwright, estructura base y configuracion lista para ejecutar."
agent: "qa-arquitecto"
argument-hint: "Indica lenguaje (JS/TS), tipo de API, ambientes, autenticacion, CI y alcance inicial de endpoints."
---

# Iniciar proyecto API con Playwright

Usa este prompt para pedir a [qa-arquitecto](../agents/qa-arquitecto.agent.md) el bootstrap de un proyecto de automatizacion de API desde cero.

## Entrada recomendada

- lenguaje: JavaScript o TypeScript
- framework objetivo: Playwright Test para API
- ambientes: local, qa, staging o prod-like
- autenticacion: token, basic, oauth o sin auth
- alcance inicial: smoke, contrato, regresion o mezcla

## Instrucciones para esta ejecucion

1. Crear la estructura minima viable para API testing desde cero.
2. Configurar Playwright para pruebas API puras, con manejo de variables en `.env` y `.env.example`.
3. Definir convenciones de carpetas, naming y reutilizacion de request context/helpers.
4. Proponer e implementar un primer smoke suite de endpoints criticos.
5. Dejar scripts de ejecucion y validacion inicial listos para CI.

## Salida esperada

- archivos creados y configuraciones principales
- comandos ejecutados o sugeridos para bootstrap
- primer set de tests API funcionales
- checklist de validacion inicial
- riesgos y siguientes decisiones tecnicas

## Referencias

- [Skill QA Arquitecto](../agents/skills/qa-arquitecto/SKILL.md)
- [Skill Playwright API](../agents/skills/playwright-api/SKILL.md)
- [Skill Playwright](../agents/skills/playwright-skill/SKILL.md)
