# Auth bypass y sesión

## Cobertura mínima

- acceso directo a rutas protegidas sin autenticación
- RBAC y permisos por endpoint o acción
- IDOR sobre recursos por identificador manipulable
- logout real e invalidez de sesión previa
- manipulación básica de cookies, headers o tokens
- rechazo de requests con credenciales ausentes, vencidas o alteradas

## Secuencia recomendada

1. Enumerar roles, áreas protegidas y endpoints críticos.
2. Verificar acceso anónimo a páginas y APIs protegidas.
3. Probar usuario de menor privilegio contra rutas o acciones elevadas.
4. Intentar acceso a recursos de terceros por cambio de id.
5. Confirmar que logout, expiración y rotación de sesión cortan acceso.
6. Revisar que el sistema falla en deny por defecto ante tokens o cookies inválidas.

## Señales de hallazgo fuerte

- `200` o datos útiles donde debía existir `401` o `403`
- un usuario estándar visualiza o modifica recursos ajenos
- logout aparente pero la sesión sigue operativa
- cambios de path, casing, método o headers alteran indebidamente la autorización
- el backend acepta tokens manipulados o sesiones reusadas

## Artefactos útiles

- matriz endpoint -> rol esperado
- storage state por rol si existe automatización
- ejemplos mínimos de request y response

## Límite de esta guía

Esta fase cubre baseline funcional de authn/authz. OAuth avanzado, scanners o explotación profunda se dejan para olas posteriores.
