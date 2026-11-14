# Reglas de desarrollo — índice

> Directorio de configuración del **agente de código** (Cursor, Claude Code, Windsurf…). No es código de producto: el producto vive en `agents/` y `skills/`.
> Cada regla declara su **alcance**, para que el agente sepa cuándo aplicarla.

## Cómo se escribe una regla

Cada archivo `<rule-name>.md` en esta carpeta debe empezar con este bloque:

```markdown
---
scope: always-on | path-pattern | agent-requested
applies-to: <patrón de rutas, solo si scope es path-pattern>
---
```

| Alcance | Significado |
|---|---|
| `always-on` | El agente la aplica en toda sesión, sin excepción |
| `path-pattern` | Se activa solo cuando toca archivos que coinciden con `applies-to` |
| `agent-requested` | El agente la carga cuando la tarea lo requiere (por ejemplo, al preparar un PR) |

## Reglas de este repositorio

| Archivo | Alcance | Aplica a | De qué va |
|---|---|---|---|
| `file-and-naming-conventions.md` | always-on | — | Cómo se nombran archivos, carpetas, ramas y commits |
| `app-layer-placement.md` | path-pattern | `uis/**`, `services/**`, `agents/**`, `skills/**`, `mcps/**`, `workflows/**`, `packages/**`, `shared/**` | Dónde vive cada tipo de código y qué no se crea |
| `data-and-secrets.md` | always-on | — | Datos ficticios, secretos, explicabilidad y logs |
| `memory-bank-maintenance.md` | agent-requested | `memory-bank/**` | Cuándo y cómo se actualiza el banco de memoria |

> Reglas nuevas: crea `<rule-name>.md` con su bloque de alcance y **añádela a esta tabla**. Una regla que no se indexa aquí es invisible para el agente.

## Regla de precedencia

Si una regla entra en conflicto con `BRIEFING.md` o con `AGENTS.md`, mandan esos dos. Si el conflicto es entre dos reglas, gana la de alcance más específico (`path-pattern` sobre `always-on`). Cualquier conflicto sin resolver se registra como pregunta abierta en `memory-bank/progress.md`.
