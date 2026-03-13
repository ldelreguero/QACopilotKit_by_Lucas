# Seguridad base para apps con LLM

## Objetivo

Evaluar riesgos de aplicaciones que usan modelos, prompts, contexto recuperado o herramientas, sin asumir pentest profundo del modelo ni red teaming completo.

## Cobertura mínima

- prompt injection directa e indirecta
- fuga de datos sensibles en respuestas o contexto
- sobreconfianza en instrucciones del usuario frente a políticas del sistema
- uso indebido de tools, conectores o acciones con privilegio
- exposición de secretos, memoria o documentos por retrieval no filtrado

## Preguntas de entrada

- el sistema usa prompt del sistema, plantillas o cadenas multi-turn
- hay RAG, archivos, memoria, historial o contexto externo
- el modelo puede llamar herramientas, ejecutar acciones o tocar datos sensibles
- existen límites de rol, tenant o permisos que el modelo debe respetar

## Secuencia recomendada

1. Mapear entradas controladas por usuario y fuentes indirectas de contexto.
2. Probar override de instrucciones, exfiltración y cambio de objetivo.
3. Verificar que el modelo no ejecute herramientas fuera de política.
4. Confirmar que documentos, memoria o contexto se filtran por permiso y tenant.
5. Reportar separando fallo de guardrails, fallo de aplicación y riesgo de diseño.

## Señales de hallazgo fuerte

- el modelo revela secretos, políticas internas o contexto no destinado al usuario
- un prompt de usuario anula restricciones críticas del sistema
- una herramienta se ejecuta con parámetros no autorizados o sin validación adicional
- contenido inyectado desde documentos o páginas externas altera la conducta del sistema

## Límite de esta guía

Esta fase cubre seguridad base de aplicaciones con LLM. Evaluaciones avanzadas de jailbreak, robustness a gran escala o benchmarking especializado quedan para una ola posterior.
