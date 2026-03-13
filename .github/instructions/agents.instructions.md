---
applyTo: ".github/agents/**/*.agent.md"
---

# Guía para agentes

- Usa frontmatter YAML válido y mantén solo propiedades realmente necesarias.
- `description` debe ser específica, no genérica.
- Si defines `agents:`, incluye también la tool `agent`.
- Usa `target: vscode` cuando el agente esté pensado para el editor.
- Mantén el cuerpo del agente enfocado en comportamiento, límites, delegación y criterios de calidad.
- Evita convertir el cuerpo del agente en documentación extensa del proyecto.