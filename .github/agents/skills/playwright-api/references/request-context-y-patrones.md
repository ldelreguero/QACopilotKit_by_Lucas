# Request Context y patrones

## Estructura base sugerida

- `tests/api/`
- `fixtures/api.fixture.ts`
- `clients/<recurso>-api-client.ts`
- `data/payloads/`
- `utils/schema-validator.ts` si ya existe contrato formal

## Validaciones minimas

- status code esperado
- header `content-type`
- campos requeridos del body
- mensaje o codigo de error en escenarios negativos
- tiempo de respuesta solo si existe un umbral acordado

## Uso hibrido correcto

- prepara el estado por API
- navega por UI solo para verificar efecto visible
- evita repetir por UI lo que ya validaste por API