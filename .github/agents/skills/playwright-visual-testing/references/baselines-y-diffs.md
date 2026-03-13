# Baselines y diffs

## Controles de estabilidad

- desactivar animaciones
- esperar estado observable real
- fijar viewport y tema
- ocultar o enmascarar elementos dinamicos
- evitar capturas con skeletons o loaders activos

## Politica de baseline

- actualizar solo cuando el cambio visual sea intencional
- revisar el diff antes de aceptar snapshots nuevos
- separar cambios de contenido dinamico de cambios de layout

## Señales de regresion fuerte

- overlap o clipping de texto
- desaparicion de CTA o navegacion
- error de breakpoint
- contraste o visibilidad degradada en estados criticos