# Impacto y recorte

## Tres capas útiles

- smoke: salud básica, auth, home, endpoint crítico y flujo comercial principal
- regresión focalizada: áreas tocadas por el cambio y dependencias inmediatas
- regresión completa: cobertura amplia antes de release o cuando el riesgo transversal es alto

## Factores para subir prioridad

- cambio en componente compartido
- historial de incidentes en el área
- cambio de contrato o esquema
- integraciones externas o side effects
- corrección de bug severo que puede reabrirse

## Salida recomendada

- tests o suites incluidas
- justificación de inclusión
- riesgos no cubiertos
- recomendación final de nivel de ejecución
