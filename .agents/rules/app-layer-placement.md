---
scope: path-pattern
applies-to: uis/**, services/**, agents/**, skills/**, mcps/**, workflows/**, packages/**, shared/**, internal/**, scripts/**, infra/**
---

# Ubicación del código en la capa de aplicación

Se activa cuando la tarea toca cualquiera de las carpetas de aplicación. Su objetivo es que nunca se cree un árbol paralelo ni se ponga código en el sitio equivocado.

## 1. Dónde vive cada cosa

| Tipo de código | Ruta | Regla |
|---|---|---|
| Web corporativa pública | `uis/website` | Ruta de entrada `/`. Marca y copy alineados con `BRIEFING.md` |
| App interna | `uis/backoffice` | Layout propio, separado del sitio público. Vista de entrada en `/` |
| API o worker backend | `services/<servicio>` | Una subcarpeta por servicio, con su `README.md` |
| Agente de IA de producto | `agents/<agente>` | Una subcarpeta por agente, con objetivo, capacidades, fuentes de conocimiento, herramientas y cómo probarlo |
| Capacidad reutilizable de un agente | `skills/<skill>` | Una subcarpeta por skill |
| Servidor MCP | `mcps/<servidor>-mcp` | Una subcarpeta por servidor, documentando tools y recursos |
| Automatización o integración | `workflows/` | Documentar trigger, inputs/outputs, credenciales, pasos y errores |
| Paquete compartido versionable | `packages/<paquete>` | Una subcarpeta por paquete, con su API pública |
| Recurso compartido sin empaquetar | `shared/` | Plantillas, esquemas y assets que se copian o consultan |
| Herramienta interna con dependencias y tests | `internal/` | CLI, evaluador de prompts… |
| Script suelto de apoyo | `scripts/` | Reproducible y seguro en cualquier entorno |
| Infraestructura y despliegue | `infra/` | Docker, Terraform, manifiestos |

## 2. Qué no se crea nunca

- **Ningún árbol paralelo.** Nada de `apps/`, `frontend/`, `api/`, `backend/`, `web/`, `server/`. Los frontends van en `uis/`; los backends, en `services/`.
  > Los README de `packages/` y `shared/` mencionan una carpeta `apps/` que no existe en esta plantilla: se interpreta como `uis/` + `services/`.
- **Ninguna carpeta de primer nivel nueva** sin un ADR en `memory-bank/decisions.md` y aprobación del tech lead.
- **Código compartido dentro de un componente.** Si dos componentes lo necesitan, va a `packages/`.
- **Reglas o skills del agente de código fuera de `.agents/`.** No van en `agents/` ni en `skills/`.

## 3. Confusiones que hay que evitar

| Par | Regla |
|---|---|
| `.agents/` vs `agents/` y `skills/` | `.agents/` configura la herramienta de desarrollo. `agents/` y `skills/` son producto |
| `memory-bank/` vs `docs/` | El banco de memoria es el contexto corto y activo del agente. `docs/` son guías detalladas para personas |
| `packages/` vs `shared/` | Código que se importa o versiona → `packages/`. Archivos que se copian o consultan → `shared/` |
| `internal/` vs `scripts/` | Herramienta con dependencias y tests → `internal/`. Script suelto → `scripts/` |
| `workflows/` vs `services/` | Orquestación entre sistemas y jobs programados → `workflows/`. Lógica de negocio expuesta como API → `services/` |

## 4. Antes de crear un componente nuevo

1. Lee el `README.md` de la carpeta destino.
2. Comprueba que no existe ya un componente equivalente.
3. Crea la subcarpeta con su propio `README.md` (objetivo, tecnología y cómo ejecutarlo).
4. Regístralo en el README de la carpeta padre.
5. Si el componente implica una decisión de arquitectura, añade el ADR en `memory-bank/decisions.md`.

## Cómo verificar esta regla

- [ ] `git status --short` no muestra ninguna carpeta nueva de primer nivel.
- [ ] Ninguna ruta contiene `apps/`, `frontend/`, `api/` ni `backend/`: `git diff --staged --name-only | grep -E '(^|/)(apps|frontend|api|backend)/'` no devuelve nada.
- [ ] Todo componente nuevo tiene su `README.md` y aparece registrado en el README de su carpeta padre.
- [ ] El código nuevo vive en la ruta que indica `memory-bank/techContext.md` §3 para ese tipo de componente.

## Documentos relacionados

- `memory-bank/techContext.md` §3 — mapa completo de carpetas y límites.
- `memory-bank/product-context.md` — plan de productos y dónde vivirá cada uno.
- `AGENTS.md` §3 — áreas protegidas.
