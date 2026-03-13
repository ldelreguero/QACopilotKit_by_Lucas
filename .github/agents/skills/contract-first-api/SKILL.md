---
name: contract-first-api
description: Define y operacionaliza pruebas contract-first para consumidores y proveedores, con versionado, verificacion y puertas de CI. Usar cuando el usuario necesite Pact u otro flujo de contratos entre servicios.
license: Proprietary
compatibility: Diseñada para GitHub Copilot en VS Code con equipos que exponen contratos versionados entre servicios o consumidores externos.
metadata:
  author: Lucas del Reguero Martinez
  version: "1.0"
---

# Contract First API

## Cuando usar

Usa esta skill cuando la tarea implique:

- diseñar un flujo contract-first entre consumidor y proveedor
- generar contratos minimos desde el uso real del consumidor
- introducir Pact o verificacion equivalente en CI
- revisar compatibilidad antes de desplegar servicios

## Resultado esperado

La salida debe dejar:

1. contrato fuente y versionado definidos
2. responsabilidades separadas entre consumidor y proveedor
3. flujo de verificacion en CI o pre-merge propuesto
4. criterio claro de compatibilidad y ruptura

## Instrucciones

1. Modela el contrato desde el consumo real.
   El consumidor debe declarar solo campos, headers y estados que realmente necesita.
2. Separa generacion y verificacion.
   Los tests del consumidor publican el artefacto; el proveedor verifica sin depender del runtime del consumidor.
3. Versiona todo.
   Contrato, consumidor y proveedor deben quedar trazables para responder si un deploy es seguro.
4. Define provider states de forma estable.
   Evita estados ambiguos o dependientes de datos manuales.
5. Haz visible la ruptura.
   Remocion de campos, cambio de tipo o de semantica debe bloquear integracion.
6. Introduce la cobertura por valor.
   Empieza por integraciones criticas antes de intentar cubrir toda la superficie API.

## Recursos relacionados

- [Pact y versionado](references/pact-y-versionado.md)
- [qa-arquitecto](../qa-arquitecto/SKILL.md)
- [openapi-testing](../openapi-testing/SKILL.md)

## Casos borde

- Si el equipo no tiene broker ni versionado de artefactos, no prometas can-i-deploy real todavia.
- Si hay un solo servicio sin consumidores separados, probablemente baste con validacion de contrato y no con CDC completo.
- Si el contrato actual refleja exceso de datos, reduce la superficie antes de automatizar.