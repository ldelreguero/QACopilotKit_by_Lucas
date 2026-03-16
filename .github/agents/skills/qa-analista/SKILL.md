---
name: qa-analista
description: Analiza resultados de tests, compara JSON y payloads, detecta regresiones, valida contratos y resume impacto funcional. Usar cuando el usuario pida comparar respuestas, revisar cambios entre ejecuciones, interpretar reportes Allure o validar integridad de datos.

compatibility: Diseñada para GitHub Copilot en VS Code con acceso a reportes, JSON, resultados de tests y artefactos de ejecución.

---

# QA analista

## Cuándo usar

Usa esta skill cuando la tarea implique:

- comparar baseline vs ejecución actual
- hacer diff estructural de JSON, payloads o respuestas API
- validar contratos, esquemas o integridad de datos
- interpretar resultados de regresión, Allure o históricos de ejecución
- priorizar cambios según severidad e impacto funcional

## Resultado esperado

La salida debe incluir:

1. hallazgos principales ordenados por severidad
2. campos o comportamientos modificados
3. impacto esperado en negocio o funcionalidad
4. clasificación entre cambio esperado, regresión o riesgo
5. siguiente acción sugerida

## Instrucciones

1. Determina el tipo de comparación.
   Identifica si comparas JSON, response bodies, reportes de tests, métricas o artefactos Allure.
2. Normaliza la evidencia.
   Homologa formato, orden de claves, timestamps no determinísticos, ids efímeros y ruido ambiental antes de concluir que hay una regresión.
3. Compara estructura y contenido.
   Detecta campos agregados, eliminados, modificados, cambios de tipo, enums inválidos y desviaciones de rango.
4. Prioriza por impacto.
   Trata como más grave lo que rompe compatibilidad, elimina campos requeridos, cambia tipos o altera semántica de negocio.
5. Valida contrato si existe esquema.
   Revisa required, types, enum, límites y additionalProperties.
6. Distingue diferencia de regresión.
   Un cambio no es regresión si está alineado con un cambio intencional documentado o con una evolución compatible del contrato.
7. Resume accionablemente.
   Explica qué cambió, por qué importa y qué debe revisar el equipo.

## Criterios de severidad

- Critical: campo requerido ausente, tipo incompatible, contrato roto, pérdida de datos o imposibilidad de consumo
- High: cambio semántico importante, status inesperado, valor inválido en campo crítico
- Medium: campo nuevo no documentado, cambio compatible con impacto potencial, desviación de rango no bloqueante
- Low: diferencias cosméticas, orden, formato o metadatos sin efecto funcional

## Reglas para diff JSON

- Señala explícitamente `ADDED`, `REMOVED`, `CHANGED` y `TYPE_CHANGED`.
- Usa rutas completas para cada diferencia, por ejemplo `user.profile.email`.
- Si hay arrays, aclara si el cambio es de longitud, orden o contenido.
- Si la comparación incluye valores sensibles o grandes blobs, resume sin reproducir ruido innecesario.

## Reglas para validación de contrato

- Verifica campos requeridos antes de revisar el resto.
- Si `additionalProperties` es `false`, reporta campos inesperados.
- Si un enum o rango falla, indica valor recibido y restricción esperada.
- Si no hay schema formal, deriva un contrato implícito desde el baseline y declara esa limitación.

## Interpretación de reportes de test

- Diferencia entre fallo nuevo, fallo conocido, flaky y fallo ambiental.
- Agrupa por suite, endpoint, feature o tipo de error cuando haya volumen alto.
- Si hay Allure u otro reporte, resume el total, fallidos, skipped, tendencia y áreas más afectadas.
- No presentes un resumen plano si los datos permiten aislar una regresión concreta.

## Formato de salida recomendado

Usa esta tabla si la comparación tiene varias diferencias:

`Severidad | Tipo | Path | Valor anterior | Valor actual | Impacto`

Y cierra con:

- Veredicto: `sin cambios relevantes`, `cambio esperado`, `riesgo`, o `regresión confirmada`
- Acción siguiente: validar contrato, corregir API, actualizar baseline o investigar origen del cambio

## Recursos relacionados

- [Plantillas de diff y reporte](references/plantillas-de-analisis.md)
- [Allure y tendencias](references/allure-y-tendencias.md)
- [Validacion de contratos API](references/validacion-de-contratos-api.md)
- [observability-analysis](../observability-analysis/SKILL.md)
- [regression-selection](../regression-selection/SKILL.md)

## Casos borde

- Si faltan baseline o expected data, pide o infiere un baseline provisional y márcalo como tal.
- Si las diferencias dependen de datos aleatorios, elimina primero la variabilidad antes de concluir.
- Si hay múltiples cambios en cascada, separa causa primaria de efectos derivados.