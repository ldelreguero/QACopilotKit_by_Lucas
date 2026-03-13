# Patrones y plantilla de salida

## Checklist rapido de lectura

- Confirmar servicio, ambiente, rango temporal y origen del log.
- Detectar primer error significativo antes de revisar errores derivados.
- Agrupar por request-id, trace-id, span-id, host, pod o thread cuando existan.
- Separar evidencia observada de hipotesis.
- Declarar limitaciones si faltan timestamps, contexto o continuidad.

## Firmas utiles

- OOMKilled, Java heap space, Cannot allocate memory: problema de memoria o presion de recursos.
- ETIMEDOUT, Gateway Timeout, SocketTimeoutException: timeout de red o dependencia degradada.
- ECONNREFUSED, Connection refused: servicio destino caido o puerto incorrecto.
- TLS handshake failure, certificate expired: error de certificados o configuracion TLS.
- AccessDenied, Permission denied, 403: permisos, IAM o RBAC.
- duplicate key, constraint violation: problema de datos o concurrencia.
- readiness probe failed, crash loop: despliegue o dependencia de arranque.

## Plantilla de salida

### Resumen tecnico

- Causa raiz mas probable:
- Evidencia principal:
- Cadena de propagacion:
- Impacto:
- Criticidad:
- Certeza:

### Acciones

- Inmediatas:
- De validacion:
- Estructurales:

## Cadena de error

Usar cuando haya varias capas:

`[evento disparador] -> [evento derivado] -> [fallo visible]`
