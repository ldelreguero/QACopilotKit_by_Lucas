# SKILL — Analista Universal de Logs

## Objetivo

Esta skill define las capacidades técnicas, heurísticas de análisis y clasificaciones de errores que utiliza el agente `analista-universal-logs` durante el análisis forense de logs.

---

## 1. Capacidades Técnicas

### 1.1 Análisis de Stack Traces

- Identificar la excepción raíz (`Caused by`, `Root cause` o equivalentes).
- Seguir la cadena de invocación desde el punto de entrada hasta el fallo.
- Distinguir frames de aplicación vs framework/librería.
- Detectar recursión infinita o profundidad anómala de stack.
- Detectar excepciones envueltas y priorizar la más interna.

### 1.2 Identificación de Eventos Críticos

- Detectar niveles `ERROR`, `FATAL`, `CRITICAL`, `SEVERE`, `PANIC` y equivalentes.
- Identificar shutdowns no planificados, restarts y crashes.
- Reconocer señales de terminación (`SIGKILL`, `SIGTERM`, `OOMKilled`).
- Detectar assertions y health checks fallidos.

### 1.3 Análisis de Timestamps

- Construir línea temporal precisa de eventos.
- Detectar gaps temporales anómalos.
- Calcular duraciones y comparar con umbrales razonables.
- Identificar ráfagas de errores.
- Detectar desincronización de relojes entre servicios.

### 1.4 Correlación entre Logs

- Correlacionar por `request-id`, `trace-id`, `correlation-id`, `span-id`.
- Vincular eventos multi-servicio de la misma transacción.
- Rastrear el flujo completo de un request.
- Detectar pérdida de contexto o corte de traza.
- Correlacionar logs de aplicación e infraestructura por tiempo/host.

### 1.5 Identificación de Anomalías

- Detectar desviaciones de frecuencia/formato/contenido.
- Identificar valores fuera de rango (latencia, tamaños, contadores).
- Detectar aparición de nuevos tipos de error.
- Detectar ausencia de eventos esperados.
- Identificar cambios abruptos de volumen de logging.

### 1.6 Detección de Errores Encadenados

- Determinar evento disparador (trigger event).
- Mapear cadena de propagación del error.
- Identificar fallos secundarios (consecuencia, no causa).
- Reconocer fallos en cascada.
- Diferenciar concurrencia de causalidad.

### 1.7 Análisis de Dependencias Fallidas

- Detectar fallos de conexión a APIs, DB, cache, colas.
- Identificar timeouts de dependencias.
- Reconocer circuit breakers activados.
- Detectar fallos DNS.
- Detectar respuestas 5xx, respuestas malformadas y degradación gradual.

### 1.8 Identificación de Fallos de Red

- Detectar `Connection refused/reset/timed out`.
- Identificar fallos TLS/SSL y certificados expirados.
- Reconocer `NXDOMAIN`, `SERVFAIL`, resolución lenta.
- Detectar bloqueos por firewall/security groups.
- Identificar saturación de sockets/puertos y particiones de red.

### 1.9 Detección de Problemas de Permisos

- Identificar fallos de autenticación (`401`, credenciales inválidas).
- Identificar fallos de autorización (`403`, permisos insuficientes).
- Detectar problemas de permisos de filesystem.
- Detectar tokens expirados/revocados.
- Identificar problemas IAM/RBAC.

### 1.10 Identificación de Timeouts

- Detectar timeouts de conexión, lectura, escritura y transacción.
- Detectar timeouts de health checks/probes.
- Identificar operaciones que exceden el tiempo máximo.
- Diferenciar timeout de cliente vs servidor.

### 1.11 Análisis de Recursos

- Detectar agotamiento de memoria (`OutOfMemoryError`, `OOMKilled`).
- Identificar uso elevado de CPU y saturación de recursos.
- Detectar disco lleno (`No space left on device`).
- Detectar agotamiento de file descriptors/handles.
- Identificar agotamiento de pool de conexiones, threads o procesos.
- Reconocer memory leaks y GC pauses prolongadas.

### 1.12 Configuraciones Erróneas

- Detectar configuraciones inválidas o faltantes.
- Identificar variables de entorno no definidas/incorrectas.
- Reconocer incompatibilidades de versiones.
- Detectar feature flags mal configurados.
- Identificar endpoints/puertos/hosts incorrectos.

---

## 2. Heurísticas de Análisis

### 2.1 Primer Error Significativo

Regla: el primer error significativo en secuencia temporal suele ser la causa raíz; los posteriores suelen ser efectos.

### 2.2 Última Excepción No Manejada

Regla: la última excepción no controlada antes de crash/shutdown suele marcar el punto final de fallo.

### 2.3 Cambios Previos al Error

Regla: errores inmediatamente posteriores a deploy/restart/config-change suelen estar relacionados con ese cambio.

### 2.4 Errores Repetitivos en Ventana Corta

Regla: repetición idéntica o casi idéntica suele indicar problema sistémico (retry loop, degradación).

### 2.5 Correlación por Request/Trace ID

Regla: eventos con el mismo identificador pertenecen a la misma transacción; el error primario de esa traza explica el fallo del request.

### 2.6 Correlación por Servicio

Regla: múltiples servicios fallando simultáneamente sugieren dependencia compartida fallida o evento común.

### 2.7 Consistencia de Datos

Regla: errores de formato, parsing o constraints sugieren problema en la fuente o integración de datos.

### 2.8 Impacto Cruzado

Regla: el impacto real incluye servicios dependientes directos/transitivos, no solo el servicio que emite el error.

---

## 3. Clasificación de Errores

### 3.1 Error de Aplicación

- Indicadores: `NullPointerException`, `TypeError`, assertions fallidas.
- Capa: Aplicación.

### 3.2 Error de Datos

- Indicadores: `ConstraintViolationException`, `ParseException`, `duplicate key`.
- Capa: Aplicación / Base de Datos.

### 3.3 Error de Infraestructura

- Indicadores: `OOMKilled`, `No space left on device`, node/pod failures.
- Capa: Infraestructura.

### 3.4 Error de Red

- Indicadores: `ECONNREFUSED`, `ETIMEDOUT`, TLS handshake failures.
- Capa: Red.

### 3.5 Error de Permisos

- Indicadores: `401`, `403`, `AccessDenied`, `Permission denied`.
- Capa: Seguridad.

### 3.6 Error de Configuración

- Indicadores: `Missing required property`, `Invalid value`, `Unknown host`.
- Capa: Configuración.

### 3.7 Error de Dependencia Externa

- Indicadores: `502/503/504`, `Service Unavailable`, circuit breaker abierto.
- Capa: Dependencias Externas.

### 3.8 Error de Concurrencia

- Indicadores: deadlocks, lock wait timeout, `ConcurrentModificationException`.
- Capa: Aplicación / Base de Datos.

### 3.9 Error de Timeout

- Indicadores: `TimeoutException`, `SocketTimeoutException`, `Gateway Timeout`.
- Capa: Red / Aplicación / Base de Datos.

### 3.10 Error de Integración

- Indicadores: schema mismatch, serialización/deserialización fallida.
- Capa: Aplicación / Integración.

### 3.11 Error de Despliegue

- Indicadores: readiness probes fallidas, `ClassNotFound`, rollback triggered.
- Capa: Despliegue / Aplicación.

---

## 4. Capacidades Avanzadas (Modo Experto)

### 4.1 Matriz de Análisis

Siempre estructurar hipótesis en la matriz:

`Causa Potencial | Impacto | Severidad | Probabilidad | Evidencia en Log | Nivel de Certeza`

### 4.2 Criticidad

- **Crítica**: sistema caído, pérdida de datos, brecha de seguridad.
- **Alta**: funcionalidad principal afectada.
- **Media**: funcionalidad secundaria afectada.
- **Baja**: error controlado/aislado.
- **Informativa**: sin impacto operativo.

### 4.3 Cadena de Errores

Reconstruir:

`[Evento Disparador] → [Evento Derivado 1] → [Evento Derivado 2] → [Fallo Visible al Usuario]`

### 4.4 Causa Raíz (5 Whys)

Aplicar secuencia completa:
1. ¿Qué falló?
2. ¿Por qué falló?
3. ¿Por qué ocurrió esa condición?
4. ¿Por qué no se previno?
5. ¿Cuál es la causa sistémica?

### 4.5 Métricas Recomendadas

- Error rate por servicio/endpoint.
- Latencia P50/P95/P99.
- CPU/memoria/disco/file descriptors.
- Health checks, reinicios, uptime.
- Conexiones fallidas/pool saturation.
- Throughput y queue depth.
- Métricas de negocio (transacciones fallidas, tasa de éxito).

---

## 5. Patrones Transversales de Reconocimiento

- Retry Storm
- Thundering Herd
- Memory Leak
- Connection Pool Exhaustion
- Thread Starvation
- Split Brain
- Cascading Failure
- Poison Message
- Hot Partition
- Clock Skew
- Graceful Degradation Failure
- Configuration Drift

---

## Reglas de Uso de la Skill

- Basar conclusiones en evidencia explícita de logs.
- Separar hechos de inferencias.
- Señalar incertidumbre cuando falte contexto.
- Priorizar causa primaria sobre errores derivados.
- Mantener trazabilidad entre hipótesis y líneas del log.
- Generar siempre un archivo `.md` con el análisis completo además de la respuesta en chat.
- Incluir siempre una sección **Archivos Analizados** con el listado explícito de fuentes revisadas.

---

## 6. Persistencia y Trazabilidad de Entrega

- La salida final debe existir en dos canales: chat + archivo `.md`.
- El archivo debe contener el análisis completo, no un resumen parcial.
- Debe registrar qué archivo(s) fueron analizados (ruta/nombre) y el alcance (completo, parcial, rango, múltiples fuentes).
- Si no se especifica nombre, usar la convención única: `analisis-{fuente}-{YYYYMMDD-HHMM}.md`.
- `fuente` debe ser: `archivo-<nombre_base>`, `multi-log` o `entrada-chat` según corresponda.
- Todo archivo `.md` debe iniciar con la cabecera estandarizada definida en el agente.
