# OpenAPI como fuente

## Checklist previo

- el archivo abre sin errores de parseo
- las referencias `$ref` resuelven
- los schemas tienen `required` y tipos utiles
- existen responses de error relevantes

## Drift que importa

- campo requerido ausente en runtime
- tipo distinto al documentado
- enum o formato incompatible
- endpoint real sin documentar
- status code implementado pero no especificado

## Salida recomendada

- endpoints cubiertos
- schemas validados
- huecos del spec
- drift confirmado
- accion siguiente