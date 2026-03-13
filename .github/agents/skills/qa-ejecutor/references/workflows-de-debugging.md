# Workflow operativo y debugging

## Reparar un selector

1. Reproducir el fallo.
2. Inspeccionar el estado actual de la pagina.
3. Generar un locator mas estable.
4. Editar el test.
5. Reejecutar el archivo minimo.

## Prioridad de diagnostico

- selector fragil
- timing issue
- fixture incompleta
- datos inestables
- asercion obsoleta

## Comandos minimos

- `npx playwright test <archivo>`
- `npx playwright test <archivo> --headed`
- `npx playwright test <archivo> --debug`
- `npx playwright test <archivo> --trace on`

## Cierre esperado

- cambio aplicado
- comando ejecutado
- resultado observado
- limitacion remanente si no hubo validacion completa
