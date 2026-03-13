# Alcance inicial del cluster

## Línea base recomendada

- smoke de performance con baja concurrencia
- load gradual con thresholds explícitos
- stress acotado para observar degradación
- percentiles, error rate y throughput como métricas mínimas
- correlación básica con salud del sistema

## Primera ola ya consolidada

- `k6-performance` -> baseline k6 reproducible
- `artillery-load` -> Artillery para carga declarativa
- `artillery-load-testing` -> Artillery para carga declarativa

## Regla de arranque

Abrir primero con baseline reproducible y cargas progresivas. Campañas agresivas, larga duración o experimentos de capacidad vienen después.
