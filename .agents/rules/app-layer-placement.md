---
scope: path-pattern
applies-to: uis/**, services/**, agents/**, skills/**, mcps/**, workflows/**, packages/**, shared/**, internal/**, scripts/**, infra/**
---

# Ubicación del código en la capa de aplicación

Se activa cuando la tarea toca cualquiera de las carpetas de aplicación. Su objetivo es que nunca se cree un árbol paralelo ni se ponga código en el sitio equivocado.

## 1. Dónde vive cada cosa

### Rutas de este hito


| Ruta | Qué va aquí cuando se cree | Estado hoy |
|---|---|---|
| `uis/` | Proyectos frontend de la compañía, agrupados por área | Existe con `README.md`; sin proyectos |
| `uis/website` | Web corporativa pública. Ruta de entrada `/`. Marca y copy tomados de `BRIEFING.md` | No creada |
| `uis/backoffice` | App interna, con layout propio y separado del sitio público. Vista de entrada en `/` | No creada |
| `services/` | APIs y servicios backend | Existe con `README.md`; sin servicios |

### Resto de rutas

| Tipo de código | Ruta | Regla |
|---|---|---|
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

## 2. Los `README.md` de la plantilla manda

**El README de cada carpeta es la fuente de verdad sobre qué va en ella.** Antes de crear cualquier archivo o subcarpeta:

1. Lee el `README.md` de la carpeta destino.
2. Comprueba si la carpeta o el componente ya existe.
3. Sigue las instrucciones del README, no ejemplos genéricos ni suposiciones.

Si el README contradice la tabla de arriba, **manda el README** y hay que actualizar `memory-bank/techContext.md` §3.

### Qué pide el README de cada ruta

- **`uis/README.md`**: agrupa todos los proyectos con interfaz de usuario; los dos principales son `website` (presencia web pública) y `backoffice` (app interna de administración, lugar para autenticación, gestión de personas y de operaciones y comunicación interna). Organiza `uis/` por **áreas de la compañía**, con documentación técnica y funcional propia en cada subcarpeta, y **registra cada aplicación nueva en este README o en un sub-README** con su objetivo, la tecnología usada y cómo ejecutarla.
- **`services/README.md`**: contiene todas las APIs y workers backend. Cada subcarpeta es **un servicio concreto** (`admin-api`, `data-processor-worker`…) con su documentación técnica y funcional. Registra cada servicio nuevo en el README con su objetivo, la tecnología usada y cómo ejecutarlo.

## 3. Qué no se crea nunca

- **Ningún árbol paralelo.** Nada de `apps/`, `frontend/`, `api/`, `backend/`, `web/`, `server/`. Los frontends van en `uis/`; los backends, en `services/`.
  > Los README de `packages/` y `shared/` mencionan una carpeta `apps/` que no existe en esta plantilla: se interpreta como `uis/` + `services/`.
- **Ninguna carpeta de primer nivel nueva** sin un ADR en `memory-bank/decisions.md` y aprobación del tech lead.
- **Código compartido dentro de un componente.** Si dos componentes lo necesitan, va a `packages/`.
- **Reglas o skills del agente de código fuera de `.agents/`.** No van en `agents/` ni en `skills/`.

## 4. Confusiones que hay que evitar

| Par | Regla |
|---|---|
| `.agents/` vs `agents/` y `skills/` | `.agents/` configura la herramienta de desarrollo. `agents/` y `skills/` son producto |
| `memory-bank/` vs `docs/` | El banco de memoria es el contexto corto y activo del agente. `docs/` son guías detalladas para personas |
| `packages/` vs `shared/` | Código que se importa o versiona → `packages/`. Archivos que se copian o consultan → `shared/` |
| `internal/` vs `scripts/` | Herramienta con dependencias y tests → `internal/`. Script suelto → `scripts/` |
| `workflows/` vs `services/` | Orquestación entre sistemas y jobs programados → `workflows/`. Lógica de negocio expuesta como API → `services/` |

## 5. Antes de crear un componente nuevo

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
- [ ] Se ha leído y respetado el `README.md` de la carpeta destino.

## Documentos relacionados

- `memory-bank/techContext.md` §3 — mapa completo de carpetas y límites.
- `memory-bank/product-context.md` — plan de productos y dónde vivirá cada uno.
- `AGENTS.md` §3 — áreas protegidas, y §4 — dónde va el código de producto.
