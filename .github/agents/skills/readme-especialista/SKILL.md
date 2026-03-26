---
name: readme-especialista
description: "Use this skill when the user asks to create, rewrite, improve, or restructure README.md content, including onboarding, installation, usage, badges, navigation, and troubleshooting sections for the main project documentation."
---

## Propósito

Esta skill define un flujo reusable para crear o mejorar `README.md` con foco en onboarding rapido, legibilidad y utilidad real para quien clona el repositorio por primera vez.

## Cuándo activar

Activa esta skill cuando el usuario pida:

- crear un README desde cero
- refactorizar un README existente
- mejorar secciones de instalacion/uso
- agregar badges, indice o navegacion
- reescribir documentacion principal para mayor claridad

No la actives para tareas de implementacion de codigo, debugging, performance, seguridad o testing.

## Flujo recomendado

1. Revisar contexto del repositorio:
   - stack, scripts en `package.json`, estructura de carpetas y docs existentes
2. Definir audiencia principal del README:
   - usuario final, developer contributor, o ambos
3. Diseñar estructura objetivo del README:
   - Overview, Features, Getting Started, Usage, Troubleshooting (segun aplique)
4. Redactar contenido breve y escaneable:
   - encabezados claros, listas cortas, comandos verificables
5. Verificar consistencia:
   - links relativos para archivos internos y secciones sin duplicar contenido de otros documentos
6. Entregar resultado:
   - README final + supuestos + faltantes de informacion

## Estándar de contenido

- Mantener tono directo y orientado a tareas.
- Evitar exceso de emojis.
- No duplicar secciones con archivo dedicado (`LICENSE`, `CONTRIBUTING`, `CHANGELOG`); usar solo referencias breves.
- Si existe logo/icono del proyecto, usarlo en cabecera solo si mejora claridad visual.

## Formato Markdown

- Usar GFM (GitHub Flavored Markdown).
- Usar alertas de GitHub solo cuando agreguen valor:
  - `> [!NOTE]`
  - `> [!TIP]`
  - `> [!IMPORTANT]`
  - `> [!WARNING]`
  - `> [!CAUTION]`

## Referencias de estilo

Tomar inspiracion de estos READMEs (estructura y tono, no copia literal):

- https://raw.githubusercontent.com/Azure-Samples/serverless-chat-langchainjs/refs/heads/main/README.md
- https://raw.githubusercontent.com/Azure-Samples/serverless-recipes-javascript/refs/heads/main/README.md
- https://raw.githubusercontent.com/sinedied/run-on-output/refs/heads/main/README.md
- https://raw.githubusercontent.com/sinedied/smoke/refs/heads/main/README.md

## Definición de listo

El README queda listo cuando una persona nueva puede entender rapidamente:

- que resuelve el proyecto
- como instalarlo y ejecutarlo
- como usar su flujo principal
- que hacer ante errores comunes
