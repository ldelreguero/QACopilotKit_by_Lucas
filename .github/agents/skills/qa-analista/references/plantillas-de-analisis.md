# Plantillas de diff y reporte

## Normalizacion previa

- Ignorar ids efimeros, timestamps variables y orden no significativo.
- Separar cambios cosmeticos de cambios semanticos.
- Si no hay schema formal, declarar baseline implicito.

## Ejemplo de diff recomendado

- Severidad: High
- Tipo: TYPE_CHANGED
- Path: order.total
- Valor anterior: "10.50"
- Valor actual: 10.5
- Impacto: rompe consumidores string-based

## Veredictos permitidos

- sin cambios relevantes
- cambio esperado
- riesgo
- regresion confirmada

## Plantilla de resumen

### Hallazgos clave

1. Diferencia principal:
2. Riesgo funcional:
3. Alcance:

### Accion siguiente

- validar contrato
- corregir API
- actualizar baseline
- investigar origen del cambio
