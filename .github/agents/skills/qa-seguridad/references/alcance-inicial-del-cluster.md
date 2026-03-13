# Alcance inicial del cluster

## Línea base recomendada

- broken access control
- autenticación y sesión
- exposición de datos sensibles
- validación de inputs e inyección
- headers y configuraciones mínimas de seguridad

## qaskills candidatas a consolidar en la siguiente ola

- ninguna en seguridad baseline

## Primera ola ya consolidada

- `owasp-security` -> baseline OWASP reproducible
- `auth-bypass-tester` -> auth bypass y sesión
- `sql-injection-testing` -> SQL injection no destructivo

## Segunda ola ya consolidada

- `zap-security-scanner` -> ZAP baseline controlado
- `llm-security-testing` -> seguridad base para apps con LLM

## Regla de arranque

Abrir primero el baseline reproducible y no destructivo. Herramientas activas o scanning agresivo vienen después.
