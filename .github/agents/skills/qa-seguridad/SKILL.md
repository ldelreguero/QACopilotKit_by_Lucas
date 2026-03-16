---
name: qa-seguridad
description: Orquesta y ejecuta análisis QA de seguridad funcional y de aplicación, priorizando OWASP, validación de authz/authn e inputs hostiles. Usar cuando el usuario pida revisar riesgos, diseñar pruebas de seguridad o preparar un baseline seguro.
license: Proprietary
compatibility: Diseñada para GitHub Copilot en VS Code con acceso a código, endpoints, suites de test o documentación técnica del sistema bajo prueba.
metadata:
  author: Lucas del Reguero Martinez
  version: "1.0"
---

# QA seguridad

## Cuando usar

Usa esta skill cuando la tarea implique:

- planificar o revisar pruebas de seguridad de aplicación
- validar auth, permisos, headers, inputs hostiles o exposición de datos
- priorizar OWASP Top 10 como baseline
- preparar el terreno para automatización de seguridad funcional

## Resultado esperado

La salida debe dejar:

1. superficie de riesgo priorizada
2. pruebas de seguridad recomendadas por dominio
3. separación entre hallazgos críticos y endurecimiento preventivo
4. alcance explícito de lo que no se valida todavía

## Instrucciones

1. Empieza por exposición real.
   Prioriza acceso, autenticación, autorización, inyección, datos sensibles y configuraciones inseguras.
2. Distingue seguridad funcional de pentest profundo.
   Esta skill cubre baseline operativo y pruebas reproducibles, no sustituye auditorías completas.
3. Usa riesgo antes que checklist.
   OWASP ordena el trabajo, pero el sistema bajo prueba decide la prioridad real.
4. Exige evidencia reproducible.
   Todo hallazgo debe poder explicarse por endpoint, flujo, rol o payload concreto.
5. Separa producto de entorno.
   Un header faltante, una autorización rota o una sesión débil no se reportan igual.
6. Cierra con mitigación.
   Toda observación debe sugerir corrección o siguiente verificación técnica.

## Primera ola consolidada

Esta skill ya absorbe tres líneas base operativas del cluster:

- baseline OWASP para priorizar superficie y evidencia
- auth bypass para authn, authz, sesión e IDOR
- SQL injection no destructivo para inputs y consultas

## Segunda ola consolidada

Esta skill también absorbe dos extensiones de seguridad con uso condicionado:

- ZAP baseline controlado para crawling y alertas no intrusivas
- LLM security testing para prompt injection, fuga de datos y abuso de herramientas

## Flujo recomendado

1. Delimita alcance y entorno.
   Confirma roles disponibles, endpoints críticos, restricciones del ambiente y si el sistema tolera pruebas activas o solo validación segura.
2. Ejecuta baseline OWASP.
   Usa la guía de cobertura para ordenar broken access control, criptografía, inyección, logging y configuraciones inseguras.
3. Profundiza en identidad y permisos.
   Si hay login, tokens, cookies o rutas protegidas, baja a auth bypass antes de expandir a otros dominios.
4. Revisa inputs persistentes y de consulta.
   Si existen filtros, búsquedas, reportes o endpoints con query/body dinámico, corre la guía de SQL injection no destructivo.
5. Añade scanner baseline solo si el entorno lo permite.
   Si hay aplicación web navegable y ambiente aislado o controlado, usa la guía de ZAP baseline y evita active scan fuera de alcance.
6. Abre seguridad de LLM solo si el producto usa IA.
   Si existen prompts, RAG, memoria, tool calling o datos sensibles en contexto, aplica la guía de LLM security.
7. Resume por severidad y verificabilidad.
   Cierra con evidencia, impacto, alcance afectado y mitigación priorizada.

## Recursos relacionados

- [Alcance inicial del cluster](references/alcance-inicial-del-cluster.md)
- [Baseline OWASP reproducible](references/baseline-owasp-reproducible.md)
- [Auth bypass y sesión](references/auth-bypass-y-sesion.md)
- [SQL injection no destructivo](references/sql-injection-no-destructivo.md)
- [ZAP baseline controlado](references/zap-baseline-controlado.md)
- [Seguridad base para apps con LLM](references/seguridad-base-para-apps-llm.md)

## Casos borde

- Si el usuario pide explotación activa fuera del baseline QA, delimita el alcance antes de automatizar.
- Si falta entorno aislado, prioriza revisiones seguras y pruebas no destructivas.
- Si hay datos sensibles reales, evita reproducirlos en artefactos o ejemplos.
- Si el sistema no usa IA generativa, no abras la rama de LLM security por defecto.