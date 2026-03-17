---
description: "Convierte hallazgos exploratorios en charters listos para transicionar a pruebas Playwright de manera incremental."
agent: "qa-exploratorio"
argument-hint: "Comparte hallazgos, bugs observados o notas de sesion y el objetivo de automatizacion posterior."
---

# QA exploratorio a charters para Playwright

Usa este prompt para pedir a [qa-exploratorio](../agents/qa-exploratorio.agent.md) la conversion de exploracion libre en artefactos que faciliten la futura automatizacion.

## Entrada recomendada

- notas de sesion exploratoria
- bugs o comportamientos sospechosos observados
- flujo funcional afectado
- prioridad de negocio e impacto esperado

## Instrucciones para esta ejecucion

1. Organizar hallazgos por riesgo, reproducibilidad e impacto.
2. Construir charters con objetivo, tecnica, datos y criterio de salida.
3. Identificar que charters conviene automatizar primero con Playwright.
4. Sugerir precondiciones API/UI para cada charter candidato.
5. Entregar backlog incremental: exploratorio manual -> semiautomatizado -> automatizado.

## Salida esperada

- charters refinados y trazables
- matriz riesgo vs esfuerzo de automatizacion
- recomendaciones de priorizacion por valor
- insumos concretos para pasar a qa-ejecutor o qa-arquitecto

## Referencias

- [Skill Pairwise Test Design](../agents/skills/pairwise-test-design/SKILL.md)
- [Skill QA Ejecutor](../agents/skills/qa-ejecutor/SKILL.md)
- [Playwright Skill](../agents/skills/playwright-skill/SKILL.md)
