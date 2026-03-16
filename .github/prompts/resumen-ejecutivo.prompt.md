---
description: "Convierte un análisis técnico de logs en un resumen ejecutivo breve y orientado a impacto."
agent: "Analista Universal de Logs"
argument-hint: "Pega el log o el análisis base y aclara la audiencia objetivo si hace falta."
---

# Resumen ejecutivo

Usa este prompt para traducir un incidente técnico a un resumen entendible para líderes técnicos o stakeholders no técnicos.

## Entrada recomendada

- log original o análisis técnico previo
- contexto de impacto conocido
- acciones ya tomadas, si existen

## Instrucciones para esta ejecución

1. Resume qué pasó, a quién afecta y qué tan grave es.
2. Traduce la causa a lenguaje simple sin perder precisión.
3. Evita stack traces, dumps o jerga innecesaria.
4. Mantén la salida breve y accionable.
5. Si corresponde, genera un `.md` con el resumen y Archivos Analizados.

## Salida esperada

- estado del incidente
- impacto funcional o de negocio
- causa identificada en lenguaje simple
- acciones tomadas o próximas acciones
- nivel de confianza

## Referencias

- [Agente Analista Universal de Logs](../agents/analista-universal-logs.agent.md)
- [Skill Analista Universal de Logs](../agents/skills/analista-universal-logs/SKILL.md)