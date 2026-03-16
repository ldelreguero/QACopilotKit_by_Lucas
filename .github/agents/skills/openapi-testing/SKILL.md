---
name: openapi-testing
description: Usa OpenAPI como fuente para generar y validar pruebas, detectar drift y organizar cobertura por schema y endpoint. Usar cuando el usuario pida trabajar spec-first con OpenAPI o Swagger.
license: Proprietary
compatibility: Diseñada para GitHub Copilot en VS Code con especificaciones OpenAPI 3.x en YAML o JSON.
metadata:
  author: Lucas del Reguero Martinez
  version: "1.0"
---

# OpenAPI testing

## Cuando usar

Usa esta skill cuando la tarea implique:

- generar pruebas desde un archivo OpenAPI
- validar respuestas contra schemas publicados
- detectar drift entre implementacion y especificacion
- revisar evolucion de endpoints o versionado del spec

## Resultado esperado

La salida debe dejar:

1. spec identificado como fuente de verdad o con limitaciones declaradas
2. endpoints o schemas priorizados
3. estrategia de validacion por request y response
4. chequeos de compatibilidad o drift definidos

## Instrucciones

1. Verifica la calidad del spec antes de generar.
   Confirma paths, methods, schemas, required y ejemplos o declara las ausencias.
2. Resuelve por endpoint y schema.
   Relaciona cada caso con el path, metodo y response esperados.
3. Valida request y response.
   No limites la cobertura al body exitoso; incluye errores, content-type y headers clave.
4. Detecta drift explicitamente.
   Reporta campos faltantes, tipos cambiados, enums rotos y endpoints no documentados.
5. Conecta con el runner real.
   Genera salida adaptable al stack del repo en lugar de crear scaffolding aislado.
6. Usa OpenAPI para ordenar, no para cegar.
   Si la implementacion real difiere, registra el gap en vez de normalizarlo silenciosamente.

## Recursos relacionados

- [OpenAPI como fuente](references/openapi-como-fuente.md)
- [qa-arquitecto](../qa-arquitecto/SKILL.md)
- [qa-analista](../qa-analista/SKILL.md)

## Casos borde

- Si el spec no compila o tiene refs rotas, frena la generacion y prioriza sanearlo.
- Si hay versionado paralelo, separa cobertura por version para evitar falsos positivos.
- Si la API real no sigue el spec, trata el hallazgo como drift y no como detalle menor.