# WCAG y Playwright

## Secuencia minima

1. Abrir la vista critica.
2. Ejecutar axe-core si el proyecto lo tiene integrado.
3. Recorrer con `Tab` y `Shift+Tab`.
4. Activar elementos con `Enter` o `Space` segun corresponda.
5. Confirmar foco visible y retorno correcto al cerrar overlays.

## Hallazgos que merecen severidad alta

- foco perdido o atrapado
- campo sin nombre accesible
- boton o link no operable por teclado
- dialogo sin foco inicial o sin cierre accesible
- error de formulario sin asociacion al campo

## Heuristicas utiles

- usa `getByRole()` y `getByLabel()` para que el test siga la semantica esperada
- no valides solo ausencia de violaciones; valida flujo operable
- si el componente usa `div` con handlers, cuestiona primero la implementacion
