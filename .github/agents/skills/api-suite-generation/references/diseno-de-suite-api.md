# Diseño de suite API

## Esqueleto minimo

- `tests/api/<recurso>/`
- `fixtures/api.fixture.ts`
- `data/factories/`
- `data/schemas/` si existe contrato formal
- `utils/auth-helper.ts` si el acceso requiere token

## Cobertura inicial por endpoint

- caso valido
- autenticacion ausente o invalida
- payload invalido o campo requerido faltante
- recurso inexistente o conflicto

## Criterio de priorizacion

- alto uso de negocio
- alto riesgo de regresion
- endpoints compartidos por varios flujos
- operaciones de escritura o borrado