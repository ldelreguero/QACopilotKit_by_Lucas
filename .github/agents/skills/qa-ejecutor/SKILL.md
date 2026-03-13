---
name: qa-ejecutor
description: Implementa, ejecuta y depura tests Playwright, encuentra selectores estables y corrige fallos de automatización con apoyo de CLI o MCP. Usar cuando el usuario pida crear tests, correr suites, reparar selectors o debuggear ejecuciones fallidas.
license: Proprietary
compatibility: Diseñada para GitHub Copilot en VS Code con Playwright instalado; puede usar Playwright MCP y CLI si están disponibles.
metadata:
  author: Lucas del Reguero Martinez
  version: "1.0"
---

# QA Ejecutor

## Cuándo usar

Usa esta skill cuando la tarea implique:

- escribir o modificar tests UI, API o híbridos
- ejecutar suites completas o parciales
- depurar fallos de Playwright
- encontrar selectores estables
- reparar tests rotos por cambios de UI, timing o datos

## Resultado esperado

La salida debe dejar:

1. test implementado o corregido
2. estrategia de ejecución aplicada
3. hallazgo técnico si hubo fallo
4. verificación final realizada o limitación explícita

## Instrucciones

1. Identifica el tipo de tarea.
   Distingue entre creación de test, ejecución, debugging, codegen o reparación de selector.
2. Usa el patrón existente del repo.
   No inventes una estructura nueva si el proyecto ya tiene convenciones.
3. Prioriza selectores estables.
   Sigue la prioridad oficial definida en `../playwright-best-practices/references/locators.md` y aplica el gate canónico antes de cerrar el cambio.
4. Reproduce el problema antes de corregirlo.
   Si hay fallo, intenta confirmar el escenario con ejecución o inspección controlada.
5. Corrige la causa raíz.
   Decide si el problema es selector, timing, datos, estado, fixture, navegación o aserción incorrecta.
6. Verifica el arreglo.
   Reejecuta el archivo o el subconjunto mínimo necesario para confirmar el cambio.
7. Resume el resultado.
   Explica qué se cambió y por qué ahora debería ser estable.

## Flujos operativos recomendados

### Crear un test

1. leer el requerimiento o spec
2. identificar precondiciones, datos y resultado esperado
3. inspeccionar UI o endpoint si hace falta
4. escribir el test en la carpeta correcta
5. ejecutar el archivo creado

### Reparar un selector

1. reproducir el fallo
2. inspeccionar el estado actual de la página
3. generar un locator más estable
4. actualizar el test
5. reejecutar para confirmar

### Debuggear un fallo

1. reproducir el escenario exacto
2. revisar selector, waits, consola, network y estado visual
3. identificar si el problema es del test o del sistema bajo prueba
4. corregir únicamente lo necesario
5. validar el resultado

## Uso de MCP

Usa MCP cuando aporte señal real, por ejemplo:

- inspección visual de la página
- snapshot del accessibility tree
- revisión de consola o network
- verificación rápida de presencia o visibilidad de un elemento

No lo uses por rutina si el problema ya puede resolverse leyendo el test, ejecutándolo o revisando el DOM esperado.

## Uso de CLI y ejecución

Comandos típicos:

- `npx playwright test`
- `npx playwright test --project=ui`
- `npx playwright test --project=api`
- `npx playwright test <archivo>`
- `npx playwright test --debug`
- `npx playwright test --headed`
- `npx playwright test --trace on`
- `npx playwright codegen <url>`

## Señales de causa raíz frecuentes

- selector frágil: usa clases volátiles, XPath o texto dinámico
- timing issue: falta espera por estado observable real
- fixture incompleta: el usuario no está autenticado o el estado previo no existe
- datos inestables: el test depende de contenido aleatorio o mutable
- aserción obsoleta: la UI cambió legítimamente y el test no fue actualizado

## Criterios de salida

- Si el test quedó corregido, indica el comando mínimo con el que fue validado.
- Si no se pudo validar, explica exactamente qué faltó.
- Si el problema parece del producto y no del test, sepáralo explícitamente del fix propuesto.

## Recursos relacionados

- [Workflow operativo y debugging](references/workflows-de-debugging.md)
- [Playwright operativo avanzado](references/playwright-operativo-avanzado.md)
- [Playwright quality gate](../playwright-best-practices/references/quality-gate.md)
- [Locator strategy](../playwright-best-practices/references/locators.md)
- [Debugging base](../playwright-best-practices/references/debugging.md)
- [playwright-accessibility](../playwright-accessibility/SKILL.md)
- [playwright-api](../playwright-api/SKILL.md)
- [playwright-visual-testing](../playwright-visual-testing/SKILL.md)

## Casos borde

- Si la página depende de terceros inestables, aísla o mockea cuando el proyecto lo permita.
- Si un selector solo existe visualmente y no semánticamente, recomienda agregar `data-testid`.
- Si el test falla de forma intermitente, no lo cierres como resuelto sin reejecución razonable.