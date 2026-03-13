# Validacion de contratos API

## Orden de revision

1. confirmar endpoint, metodo y version del contrato
2. validar status code y headers
3. validar campos requeridos
4. revisar tipos, enums y formatos
5. detectar campos extra o removidos

## Cambios compatibles

- agregar campos opcionales
- agregar nuevos endpoints versionados
- ampliar enum solo si los consumidores lo toleran y no hay parsing estricto

## Cambios incompatibles

- remover campos existentes
- cambiar tipo o formato
- agregar required nuevos en respuestas o requests consumidos
- mover o renombrar propiedades sin versionado

## Veredicto sugerido

- compatible
- compatible con riesgo
- drift del spec
- ruptura de contrato