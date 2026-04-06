---
description: "Analiza requerimientos y genera charters exploratorios con foco en riesgos, edge cases y escenarios no triviales."
agent: "qa-exploratorio"
argument-hint: "Comparte ruta del documento, historia de usuario o flujo y define que area quieres explorar primero."
---

# QA exploratorio desde requerimientos

Usa este prompt para pedir a [qa-exploratorio](../agents/qa-exploratorio.agent.md) una estrategia exploratoria accionable desde documentacion funcional o tecnica.

## Entrada recomendada

- documento fuente: requerimientos, historia, RFC o casos de uso
- modulo o flujo a explorar
- riesgos percibidos por negocio o tecnologia
- restricciones de tiempo, entorno y datos

## Instrucciones para esta ejecucion

1. Detectar ambiguedades, huecos de definicion y asunciones ocultas.
2. Transformar hallazgos en charters exploratorios priorizados por riesgo.
3. Incluir escenarios de borde, negativos y combinaciones inusuales.
4. Definir evidencia esperada para pass/fail por charter.
5. Proponer una secuencia de ejecucion exploratoria por sesiones.
6. Si detectas invariantes, riesgos o ambiguedades duraderas, cierra con la sección exacta `Sugerencia de memoria durable (opcional)` usando la [Plantilla de sugerencia de memoria](./references/memory-suggestion-template.md).

## Salida esperada

- mapa de riesgos inicial
- lista priorizada de charters
- datos o condiciones de prueba sugeridas
- criterios de observacion y evidencia
- backlog de follow-up para automatizacion futura
- sección exacta `Sugerencia de memoria durable (opcional)` si aplica

## Referencias

- [Skill Pairwise Test Design](../agents/skills/pairwise-test-design/SKILL.md)
- [Playwright Accessibility](../agents/skills/playwright-accessibility/SKILL.md)
- [Plantilla de sugerencia de memoria](./references/memory-suggestion-template.md)
