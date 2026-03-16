---
name: pairwise-test-design
description: Diseña combinaciones de prueba pairwise o combinatorias de riesgo para reducir casos manteniendo cobertura útil. Usar cuando el usuario pida optimizar matrices de datos, parámetros o variantes de configuración.

compatibility: Diseñada para GitHub Copilot en VS Code con tablas de parámetros, reglas de negocio o combinaciones de entrada susceptibles de explosión combinatoria.

---

# Pairwise test design

## Cuando usar

Usa esta skill cuando la tarea implique:

- reducir combinaciones de prueba sin ir a exhaustivo
- diseñar matrices de datos, browsers, roles o flags
- seleccionar pares críticos entre muchos parámetros
- documentar constraints y combinaciones inválidas

## Resultado esperado

La salida debe dejar:

1. parámetros y valores modelados claramente
2. restricciones o combinaciones inválidas declaradas
3. set reducido de casos con cobertura pairwise o equivalente
4. riesgos que no quedan cubiertos por diseño

## Instrucciones

1. Modela parámetros antes de generar casos.
   No mezcles valores, reglas y heurísticas en una sola lista informal.
2. Declara constraints explícitos.
   Excluye combinaciones imposibles o sin sentido antes de recortar.
3. Usa pairwise como base, no como dogma.
   Si existe riesgo de interacción de tres factores, eleva la cobertura en ese subconjunto.
4. Mantén trazabilidad.
   Cada caso debe explicar qué pares críticos cubre o por qué existe.
5. Agrega casos de riesgo fuera del algoritmo.
   Valores extremos, vacíos, nulos o secuencias peligrosas no deben perderse por optimización.
6. Entrega una matriz accionable.
   Los casos deben poder pasarse a exploración manual o a tests automatizados sin reinterpretación ambigua.

## Recursos relacionados

- [Combinatoria práctica](references/combinatoria-practica.md)
- [qa-exploratorio.agent.md](../../qa-exploratorio.agent.md)

## Casos borde

- Si los parámetros tienen fuerte dependencia, el pairwise puro puede ser insuficiente.
- Si el dominio tiene regulación o alto riesgo, añade casos obligatorios aunque no mejoren cobertura pareada.
- Si no hay parámetros bien definidos, devuelve primero el modelo y no una matriz artificial.