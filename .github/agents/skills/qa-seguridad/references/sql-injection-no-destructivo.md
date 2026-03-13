# SQL injection no destructivo

## Objetivo

Validar si el sistema maneja entradas hostiles sin romper consultas, filtrar errores internos ni devolver datos fuera de alcance, evitando payloads destructivos en esta fase.

## Superficie típica

- búsquedas y filtros
- login y recuperación de credenciales
- listados con orden, paginación o exportación
- reportes y dashboards parametrizados
- endpoints con query params o cuerpos transformados a consulta

## Estrategia segura

1. Localizar entradas que terminen en consultas o builders dinámicos.
2. Probar payloads de error controlado, unión simple o tautologías no destructivas.
3. Confirmar que no aparecen errores SQL, `500` evitables ni cambios anómalos de volumen de datos.
4. Revisar si la respuesta revela motor, stack o fragmentos de consulta.
5. Reportar también validaciones positivas si el sistema rechaza, parametriza o neutraliza bien el input.

## Indicadores de riesgo

- errores que exponen `SQL syntax`, `PostgreSQL`, `ORA-`, `mysql` o trazas del ORM
- bypass de login o incremento inesperado de resultados
- diferencias de tiempo o comportamiento asociadas a payloads triviales
- filtros que aceptan operadores o estructuras no esperadas

## Reglas de seguridad operativa

- no usar payloads destructivos como `DROP`, `DELETE` o cambios persistentes
- no correr pruebas masivas si no hay entorno aislado
- evitar inputs que alteren datos reales o saturen consultas costosas

## Salida esperada

Para cada punto evaluado: input probado, respuesta observada, evidencia de saneamiento o falla, impacto y mitigación sugerida.
