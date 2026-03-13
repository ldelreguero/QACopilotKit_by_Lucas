---
name: analista-universal-logs
description: Analiza logs técnicos, stack traces y eventos distribuidos para identificar causa raíz, cadena de propagación, impacto y criticidad. Usar cuando el usuario comparta logs, errores operativos, incidentes, reinicios, timeouts o fallos entre servicios.
license: Proprietary
compatibility: Diseñada para GitHub Copilot en VS Code con acceso a archivos de logs, búsquedas y herramientas de lectura.
metadata:
  author: Lucas del Reguero Martinez
  version: "1.0"
---

# Analista Universal de Logs

## Cuándo usar

Usa esta skill cuando la solicitud implique:

- análisis forense de logs o stack traces
- identificación de causa raíz o evento disparador
- correlación entre servicios usando request-id, trace-id, span-id o correlation-id
- explicación de caídas, reinicios, timeouts, errores 5xx, OOM, problemas de red o permisos
- producción de reportes técnicos o ejecutivos a partir de evidencia de logs

## Resultado esperado

La respuesta debe terminar con:

1. causa raíz más probable
2. evidencia principal encontrada en los logs
3. cadena de propagación del fallo
4. impacto operativo
5. criticidad y nivel de certeza
6. acciones recomendadas inmediatas y sistémicas

## Instrucciones

1. Normaliza el input.
   Identifica formato, servicio, rango temporal, timezone, hostname, ambiente y si hay múltiples fuentes.
2. Reconstruye la línea temporal.
   Ordena eventos por tiempo y marca gaps, ráfagas, reinicios y cambios previos al fallo.
3. Encuentra el primer error significativo.
   Prioriza el primer evento causal por encima de errores derivados o repetitivos.
4. Aísla la excepción raíz.
   En stack traces, prioriza el Caused by más interno o la excepción que primero rompe el flujo.
5. Correlaciona contexto.
   Agrupa por request-id, trace-id, span-id, host, pod, thread o usuario cuando existan.
6. Diferencia causa de consecuencia.
   No confundas timeouts secundarios, circuit breakers o errores en cascada con el trigger original.
7. Clasifica el incidente.
   Usa una categoría principal y, si aplica, una secundaria: aplicación, datos, infraestructura, red, permisos, configuración, dependencia externa, concurrencia, timeout, integración o despliegue.
8. Estima impacto.
   Determina qué servicio falla, qué usuarios o transacciones se afectan y si hay pérdida de datos, degradación o indisponibilidad total.
9. Expón evidencia concreta.
   Cita patrones, mensajes, timestamps y correlaciones relevantes; evita afirmar sin respaldo observable.
10. Propón mitigación.
   Separa acciones inmediatas de remediaciones estructurales.

## Heurísticas obligatorias

- El primer error significativo suele ser más valioso que el último error visible.
- La última excepción no manejada antes de un crash marca el punto de fallo visible, no siempre la causa raíz.
- Errores inmediatamente posteriores a deploy, restart o config-change deben investigarse primero desde ese cambio.
- Repetición idéntica en ventana corta sugiere problema sistémico, retry loop o degradación sostenida.
- Múltiples servicios fallando al mismo tiempo suelen apuntar a dependencia compartida, red o configuración común.
- La ausencia de un evento esperado también es evidencia.

## Capacidades técnicas a aplicar

### Stack traces

- distinguir frames de aplicación frente a framework o librería
- detectar recursión infinita, wrapping de excepciones y profundidad anómala
- identificar clase, método o módulo donde se rompe el flujo

### Eventos críticos

- detectar ERROR, FATAL, CRITICAL, SEVERE, PANIC y equivalentes
- identificar shutdowns no planificados, restarts, crashes, probes fallidas y assertions

### Tiempo y correlación

- construir timeline precisa
- detectar gaps, clock skew y ráfagas de error
- correlacionar eventos multi-servicio por ids y por proximidad temporal

### Dependencias y red

- distinguir connection refused, reset, timed out, TLS failures, DNS failures y circuit breaker open
- identificar 502, 503, 504, respuestas malformadas y degradación gradual

### Recursos, permisos y configuración

- detectar OOMKilled, No space left on device, pool exhaustion, thread starvation y GC prolongado
- identificar 401, 403, AccessDenied, Permission denied, tokens expirados o IAM/RBAC incorrecto
- reconocer variables faltantes, hosts erróneos, puertos mal configurados, feature flags y drift de configuración

## Clasificación base

| Categoría | Indicadores comunes |
|---|---|
| Aplicación | NullPointerException, TypeError, assertion failed |
| Datos | ParseException, duplicate key, constraint violation |
| Infraestructura | OOMKilled, pod failure, disk full |
| Red | ECONNREFUSED, ETIMEDOUT, TLS handshake failure |
| Permisos | 401, 403, AccessDenied, Permission denied |
| Configuración | Missing property, invalid value, unknown host |
| Dependencia externa | 502/503/504, Service Unavailable, circuit breaker |
| Concurrencia | deadlock, lock wait timeout, ConcurrentModificationException |
| Timeout | TimeoutException, SocketTimeoutException, Gateway Timeout |
| Integración | schema mismatch, deserialización fallida |
| Despliegue | readiness failure, ClassNotFound, rollback triggered |

## Formato de salida recomendado

Usa esta matriz cuando el caso sea complejo:

`Causa potencial | Impacto | Severidad | Probabilidad | Evidencia | Nivel de certeza`

Y sintetiza la cadena de errores así:

`[Evento disparador] -> [Evento derivado 1] -> [Evento derivado 2] -> [Fallo visible]`

## Criticidad

- Crítica: sistema caído, pérdida de datos o brecha de seguridad
- Alta: funcionalidad principal afectada
- Media: funcionalidad secundaria afectada
- Baja: error controlado o aislado
- Informativa: sin impacto operativo directo

## Recursos relacionados

- [Patrones y plantilla de salida](references/patrones-y-plantilla.md)

## Casos borde

- Si faltan timestamps o están mezclados, declara la limitación antes de inferir causalidad.
- Si hay múltiples hipótesis plausibles, ordénalas por evidencia y certeza, sin forzar una única explicación.
- Si los logs están truncados, especifica qué evidencia falta para confirmar la conclusión.
- Si el problema parece sistémico pero los logs son locales, indica que la visión es parcial.