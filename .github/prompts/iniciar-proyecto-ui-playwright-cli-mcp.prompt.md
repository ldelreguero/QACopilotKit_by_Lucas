---
description: "Inicia desde cero un proyecto de automatizacion UI usando Playwright CLI y Playwright MCP, con estructura lista para escalar."
agent: "qa-arquitecto"
argument-hint: "Indica tipo de app web, URL base, lenguaje (JS/TS), navegadores objetivo, CI y criterios de calidad iniciales."
---

# Iniciar proyecto UI con Playwright CLI + MCP

Usa este prompt para pedir a [qa-arquitecto](../agents/qa-arquitecto.agent.md) la creacion de un proyecto UI desde cero combinando Playwright CLI con MCP.

## Entrada recomendada

- stack frontend de la aplicacion bajo prueba
- URL base y ambientes
- lenguaje objetivo: JavaScript o TypeScript
- alcance inicial: smoke, auth, navegacion, formularios
- restricciones: CI, reporteria, secrets y politicas del equipo

## Instrucciones para esta ejecucion

1. Inicializar proyecto y dependencias para automatizacion UI con Playwright.
2. Configurar Playwright CLI para generar, ejecutar y depurar tests de forma estandar.
3. Configurar `.vscode/mcp.json` para usar Playwright MCP en inspeccion y debugging.
4. Crear estructura base (`tests`, `pages`, `fixtures`, `utils`, `data`) alineada al repo.
5. Dejar al menos un flujo smoke ejecutable y documentar como expandir cobertura.

## Salida esperada

- estructura de proyecto y archivos base creados
- configuracion de Playwright CLI y MCP aplicada
- comandos de ejecucion local y en CI
- primer test UI de referencia funcionando
- checklist de salud del proyecto

## Referencias

- [Skill QA Arquitecto](../agents/skills/qa-arquitecto/SKILL.md)
- [Playwright Skill](../agents/skills/playwright-skill/SKILL.md)
- [Playwright Test Planner](../agents/playwright-test-planner.agent.md)
