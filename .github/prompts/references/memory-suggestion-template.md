# Plantilla de sugerencia de memoria

Usa esta salida solo cuando el prompt produzca un hallazgo durable y reusable.

```md
## Sugerencia de memoria durable (opcional)
- bucket: `project-context.jsonl | user-preferences.jsonl | lessons-learned.jsonl`
- key: `dominio.subdominio.hecho`
- category: `decision | constraint | baseline | preference | lesson | runbook | standard`
- summary: `Hecho durable en una sola frase.`
- when_to_use: `Cuando esta memoria debe influir en una tarea futura.`
```

## Criterios de uso

- no completar la sección si no hay hallazgo durable
- usar `project-context.jsonl` para decisiones, restricciones y baselines del workspace
- usar `user-preferences.jsonl` para preferencias estables del usuario o equipo
- usar `lessons-learned.jsonl` para fixes reusables, causas raíz repetidas y runbooks breves
- evitar keys genéricas como `general.note` o `misc.issue`
