# Artillery para carga declarativa

## Objetivo

Usar Artillery como complemento cuando convenga describir flujos de carga con YAML o cubrir escenarios HTTP, WebSocket o eventos donde una definición declarativa simplifica la suite.

## Cuándo elegir Artillery

- escenarios HTTP con múltiples pasos declarativos
- flujos con WebSocket, eventos o secuencias breves modelables en YAML
- equipos que necesiten editar perfiles de carga sin tocar demasiada lógica de script

## Valor dentro del cluster

Artillery no reemplaza el baseline principal del dominio. En esta primera ola se integra como herramienta complementaria para escenarios donde su modelo declarativo aporta claridad.

## Secuencia recomendada

1. Delimitar flujo y volumen objetivo.
2. Definir fases simples de warm-up, steady state y cooldown.
3. Declarar checks y criterios mínimos de error.
4. Revisar resultados antes de expandir concurrencia o duración.

## Riesgos comunes

- usarlo como sustituto genérico sin justificar por qué no alcanza k6
- crecer demasiado rápido el volumen sin una corrida smoke previa
- reportar métricas sin asociarlas a fase o escenario

## Salida esperada

Escenario declarativo recomendado, motivo de elección frente a k6, métricas observadas y siguiente ampliación segura.
