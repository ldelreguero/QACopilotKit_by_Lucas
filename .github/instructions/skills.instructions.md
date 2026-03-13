---
applyTo: ".github/agents/skills/**/SKILL.md"
---

# Guía para skills

- Cada skill debe vivir en `.github/agents/skills/<skill-name>/SKILL.md`.
- El `name` del frontmatter debe coincidir exactamente con el directorio y usar lowercase-hyphen.
- La `description` debe explicar qué hace la skill y cuándo se activa.
- Mantén `SKILL.md` enfocado y mueve el detalle extenso a `references/`, `scripts/` o `assets/`.
- Usa rutas relativas al directorio de la skill para enlazar recursos.
- Evita meter múltiples skills en un mismo archivo.