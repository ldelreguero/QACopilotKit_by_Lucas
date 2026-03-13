# Tooling API condicional

## Objetivo

Dejar explícito cuándo conviene usar tooling API fuera del stack activo principal, sin abrir un dominio nuevo ni duplicar las skills basadas en Playwright y contratos.

## Postman

Úsalo cuando el equipo ya trabaja con colecciones como artefacto operativo, documentación viva o handoff entre QA y negocio.

No lo promuevas como baseline por defecto si el stack activo ya cubre API con Playwright, contratos u OpenAPI.

## Newman

Úsalo solo cuando existan colecciones Postman reales que deban correr en CI o en pipelines heredados.

No justifica una skill activa propia si el repo no depende de Newman como estándar del equipo.

## REST Assured

Úsalo solo si existe stack Java activo o un ecosistema donde mantener tests API en Java sea una restricción real.

Si el repo es principalmente Node.js o Playwright, déjalo fuera del árbol activo y trátalo como tooling condicional.

## Regla de decisión

1. Si la organización ya tiene artefactos Postman o suites Java vivas, intégralos como excepción consciente.
2. Si no existe dependencia real del stack, mantén el árbol activo centrado en las skills API ya abiertas.
3. Evita abrir un agente o skill nuevos solo por paridad de tooling.

## Salida esperada

Decisión explícita de adoptar, mantener fuera o tratar como puente temporal, con justificación de stack y costo de mantenimiento.
