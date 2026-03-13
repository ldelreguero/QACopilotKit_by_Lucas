# Baseline OWASP reproducible

## Objetivo

Usar OWASP Top 10 como orden de priorización, no como checklist ciego. La meta inicial es detectar exposición real con evidencia reproducible y bajo riesgo operativo.

## Prioridad recomendada

1. broken access control
2. autenticación y gestión de sesión
3. exposición de datos sensibles y fallas criptográficas visibles
4. inyección y validación de inputs
5. configuraciones inseguras y headers críticos
6. logging y monitoreo de eventos de seguridad

## Qué validar primero

- rutas o endpoints admin accesibles por usuarios estándar
- recursos de otro usuario accesibles por id directo o manipulable
- respuestas que devuelven datos sensibles, tokens o secretos innecesarios
- entradas de búsqueda, filtros, exportaciones o reportes que aceptan payloads hostiles
- cookies de sesión, redirecciones de login, logout y expiración
- ausencia de controles mínimos como `HttpOnly`, `Secure`, `SameSite` o headers de endurecimiento

## Evidencia mínima por hallazgo

- rol o identidad usada
- endpoint, pantalla o flujo afectado
- request o paso reproducible
- respuesta observada
- impacto técnico breve

## Qué no hacer en esta fase

- scans agresivos sin entorno aislado
- payloads destructivos o de denegación de servicio
- explotación profunda sin confirmar primero el control roto

## Cierre esperado

Entregar superficie priorizada con severidad razonada y separar hallazgos confirmados de endurecimientos preventivos.
