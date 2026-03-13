# Smoke en producción

## Regla operativa

Si la suite tarda demasiado o modifica datos de forma riesgosa, dejó de ser smoke.

## Señales que deben bloquear

- health endpoint caído
- home o login no cargan
- flujo comercial crítico roto
- error 5xx sostenido en endpoint núcleo

## Señales que pueden alertar sin bloquear

- degradación moderada de latencia
- tercero opcional inestable
- check no crítico de contenido secundario

## Diseño seguro

- usar cuentas dedicadas
- no disparar pagos reales
- no enviar emails o SMS reales
- limpiar o aislar cualquier dato temporal cuando aplique
