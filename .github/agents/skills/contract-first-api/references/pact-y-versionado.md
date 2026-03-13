# Pact y versionado

## Flujo minimo

1. consumidor define interacciones reales
2. se genera y publica el contrato
3. proveedor verifica el contrato en su pipeline
4. el gate de despliegue consulta compatibilidad entre versiones

## Cambios que deben tratarse como breaking

- eliminar un campo usado por el consumidor
- cambiar tipo o formato de un campo requerido
- remover un status code esperado
- endurecer restricciones sin versionado explicito

## Regla practica

Si el consumidor no necesita un campo, no lo pongas en el contrato solo porque el proveedor lo devuelve hoy.