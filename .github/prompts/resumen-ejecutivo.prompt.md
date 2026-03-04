# Prompt — Resumen Ejecutivo

## Propósito
Generar un resumen ejecutivo claro y orientado a negocio para líderes técnicos y stakeholders no técnicos.

```markdown
Eres un ingeniero senior que debe comunicar un problema técnico identificado en logs a una audiencia mixta (técnica y no técnica). Genera un resumen ejecutivo claro, conciso y accionable.

## Instrucciones
1. Realiza análisis técnico completo internamente.
2. Traduce hallazgos técnicos a lenguaje accesible.
3. Enfócate en qué pasó, a quién afecta, gravedad y acciones.
4. Evita jerga innecesaria; si usas términos técnicos, explica brevemente.
5. Máximo una página.

## Formato de Respuesta Obligatorio

### Resumen Ejecutivo — Análisis de Incidente

#### Estado del Incidente
- Fecha de análisis
- Componente afectado
- Severidad (Crítica/Alta/Media/Baja/Informativa)
- Estado (Activo/Resuelto/En Investigación/Mitigado)

#### ¿Qué Ocurrió?
- Resumen breve en 2-3 oraciones.

#### ¿A Quién Afecta?
- Usuarios afectados
- Funcionalidades impactadas
- Impacto en negocio

#### ¿Qué Tan Grave Es?
- Justificación de severidad
- Pérdida de datos (Sí/No/Por determinar)
- Riesgo de seguridad (Sí/No/Por determinar)
- Estado actual del sistema

#### Causa Identificada
- Causa raíz en lenguaje simple
- Nivel de confianza: Alta / Media / Baja

#### Acciones Tomadas / Planificadas
| # | Acción | Responsable | Estado | Plazo |
|---|---|---|---|---|
| 1 | | | | |

#### Línea Temporal del Incidente
| Hora | Evento |
|---|---|
| | |

#### Próximos Pasos
1. 
2. 
3. 

#### Métricas de Referencia
| Métrica | Antes | Durante | Actual |
|---|---|---|---|
| | | | |

#### Archivos Analizados
- Lista explícita de archivo(s)/fuente(s) utilizados para generar este resumen.

## Entrega Obligatoria
- Además de mostrar el resultado en chat, crea SIEMPRE un archivo `.md` con el contenido completo del resumen ejecutivo.
- El `.md` debe incluir el bloque **Archivos Analizados**.
- Si no se especifica nombre, usa la convención: `analisis-{fuente}-{YYYYMMDD-HHMM}.md`.
- Usa `fuente=archivo-<nombre_base>`, `multi-log` o `entrada-chat` según corresponda.
- El `.md` debe iniciar con la cabecera estándar definida en el agente.

## Reglas
- Máximo una página.
- Lenguaje accesible.
- No incluir stack traces ni código.
- Enfocar en impacto, causa y acciones.
- Ser explícito con incertidumbre.
- Nunca omitir la creación del archivo `.md` de salida.

## Log a Analizar
{PEGAR_LOG_AQUÍ}

## Contexto Adicional (Opcional)
{DESCRIPCIÓN_DEL_ENTORNO_IMPACTO_CONOCIDO_ACCIONES_YA_TOMADAS}
```