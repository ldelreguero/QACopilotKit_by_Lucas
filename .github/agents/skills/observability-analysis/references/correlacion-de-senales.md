# Correlación de señales

## Orden recomendado

1. identificar el flujo o request disparador
2. ubicar el trace o span raíz
3. revisar spans hijos con error, latencia o atributos faltantes
4. correlacionar con logs o métricas del mismo intervalo
5. clasificar si la anomalía es funcional, operativa o de instrumentación

## Hallazgos de alto valor

- trace cortado entre servicios
- span con status ok pero log de error real
- latencia anómala sin error explícito
- atributos críticos ausentes para triage
- errores repetidos sin métrica o alerta asociada
