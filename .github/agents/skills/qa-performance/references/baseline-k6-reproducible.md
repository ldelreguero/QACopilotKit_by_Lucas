# Baseline k6 reproducible

## Objetivo

Usar k6 como herramienta principal para establecer un baseline repetible de performance con scripts versionables, thresholds claros y perfiles graduales.

## Cuándo priorizar k6

- APIs HTTP con criterios claros de latencia y error rate
- flujos web o backend que necesiten percentiles comparables entre corridas
- campañas smoke, load, stress o soak con control fino del perfil

## Secuencia recomendada

1. Elegir endpoints o flujos representativos.
2. Definir thresholds antes de ejecutar.
3. Correr smoke breve para validar script y entorno.
4. Escalar a load gradual y recién luego a stress o soak.
5. Comparar percentiles y error rate contra el objetivo declarado.

## Métricas mínimas

- `p95` y, cuando importe, `p99`
- `http_req_failed` o equivalente
- throughput o iterations por unidad de tiempo
- tiempos por flujo crítico si existen grupos o tags

## Reglas de seguridad operativa

- no disparar concurrencia alta sin confirmar límites del ambiente
- evitar mezclar cambios funcionales y campañas de carga en la misma corrida de diagnóstico
- registrar siempre el perfil exacto ejecutado para permitir comparación posterior

## Salida esperada

Perfil aplicado, thresholds declarados, métricas observadas, degradación detectada y siguiente hipótesis de ajuste.
