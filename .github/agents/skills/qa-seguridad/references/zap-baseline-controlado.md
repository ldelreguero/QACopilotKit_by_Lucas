# ZAP baseline controlado

## Objetivo

Usar ZAP como apoyo de descubrimiento y baseline pasivo o de baja intrusión, no como sustituto del análisis funcional ni como active scan por defecto.

## Cuándo sí usarlo

- existe una web navegable o API web expuesta
- el entorno es aislado, controlado o explícitamente apto para scanning
- se busca descubrir headers, cookies, formularios, rutas y alertas obvias

## Cuándo no usarlo todavía

- el entorno es productivo o sensible y no hay autorización clara
- el usuario solo pidió baseline manual o revisión de código
- la superficie principal es un backend interno sin flujo HTTP navegable

## Modo recomendado en esta ola

1. baseline pasivo o controlado
2. spider acotado a rutas seguras
3. revisión manual de alertas antes de reportar
4. correlación con hallazgos de auth, headers y exposición de datos

## Evidencia útil

- URL o dominio escaneado
- alcance exacto permitido
- alertas priorizadas por severidad y confianza
- falsos positivos descartados de forma explícita
- relación entre alerta automática y evidencia funcional observada

## Regla de seguridad operativa

No habilitar active scan, fuzzing agresivo ni autenticaciones destructivas salvo que el entorno y el usuario lo autoricen de manera explícita.

## Salida esperada

Resumen corto de superficie descubierta, alertas confiables, falsos positivos relevantes y verificación manual pendiente.
