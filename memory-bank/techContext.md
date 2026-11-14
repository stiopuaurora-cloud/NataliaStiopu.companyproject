# Tech Context — Nexova Solutions

> Stack, arquitectura del monorepo y restricciones técnicas. Para el *porqué* de cada decisión, ver `decisions.md`; aquí está el *qué*.
> Lo que aún no se ha decidido aparece como **por definir**. No lo inventes: pregunta, o regístralo antes en `decisions.md`.
> **Última actualización:** 2026-11-14

## 1. Sistemas existentes (legacy)

| Sistema | Uso | Estado |
|---|---|---|
| **HubSpot** | CRM de Ventas | Solo el 40 % del equipo lo actualiza con regularidad |
| **Zendesk** (legacy) | Helpdesk de soporte externalizado | Sin base de conocimiento conectada |
| **ATS propio** (años 2010) | Seguimiento de candidatos | Construido internamente; sin documentación conocida |
| **Google Workspace** | Email, Drive, Forms y Sheets | Es el "pegamento" entre todo lo demás |
| Hojas de cálculo | Inscripciones, vacaciones, asistencia… | Actúan como sistemas de gestión |

**Carencias actuales:** no hay telemetría, ni logging centralizado, ni monitorización ni alertas, y los despliegues son manuales. Cuando algo falla, el equipo se entera porque avisan los usuarios.

## 2. Stack tecnológico

| Capa | Tecnología | Estado |
|---|---|---|
| Web pública (`uis/website`) | Por definir. Requisitos ya fijados: HTML semántico, SEO/GEO, schema.org, accesibilidad y responsive | Por definir |
| Backoffice (`uis/backoffice`) | Por definir | Por definir |
| Servicios (`services/`) | Por definir | Por definir |
| Agentes de producto (`agents/`) | Por definir | Por definir |
| Servidores MCP (`mcps/`) | Model Context Protocol | Por definir |
| Automatizaciones (`workflows/`) | Por definir (la plantilla apunta a herramientas como n8n) | Por definir |
| Base de datos | Por definir | Por definir |
| Vector store para RAG | Por definir | Por definir |
| LLM / proveedor de IA | Por definir | Por definir |
| Telemetría y logging | Por definir. Requisito: logs estructurados desde el primer servicio | Por definir |
| CI/CD | Por definir. Hoy los despliegues son manuales | Por definir |

## 3. Arquitectura del monorepo

Mapa de carpetas y destino de cada una, según el `README.md` de la propia plantilla. **Antes de crear contenido en cualquier carpeta, vuelve a leer su README**; si entra en conflicto con esta tabla, manda el README.

| Carpeta | Qué va | Regla |
|---|---|---|
| `uis/website` | Web corporativa pública (ruta `/`) | Marca y copy alineados con `BRIEFING.md` |
| `uis/backoffice` | App interna, layout propio y separado del sitio público | Vista de entrada en `/` |
| `services/` | APIs y workers backend (APIs y workers en segundo plano) | Una subcarpeta = un servicio, con su propia documentación técnica y funcional |
| `agents/` | Agentes de IA de producto | Una subcarpeta = un agente, con su README |
| `skills/` | Capacidades reutilizables de esos agentes | Una subcarpeta por skill |
| `mcps/` | Servidores MCP que conectan los modelos con sistemas internos | Una subcarpeta = un servidor |
| `workflows/` | Automatizaciones, jobs y sincronizaciones | Documentar triggers, inputs/outputs y errores |
| `packages/` | Paquetes compartidos versionables | Una subcarpeta = un paquete, con su API pública |
| `shared/` | Recursos compartidos sin empaquetar (plantillas, esquemas) | Documentar cada archivo |
| `internal/` | Herramientas del equipo con tests y dependencias propias | Documentar instalación y uso |
| `scripts/` | Scripts de ayuda sueltos | Reproducibles y seguros |
| `infra/` | Infraestructura y despliegue | Documentar cómo provisionar y desplegar |
| `docs/` | Documentación transversal para personas | Organizada por temas |
| `.agents/` | Reglas y skills del **agente de código** | No es código de producto |
| `memory-bank/` | Contexto persistente del proyecto | Se actualiza en cada PR |

**Los `README.md` de la plantilla son la fuente de verdad** sobre qué va en cada carpeta. Antes de crear archivos o subcarpetas nuevas, léelos. Si un README contradice esta tabla, manda el README y hay que actualizar esta tabla.

- **`uis/README.md`**: agrupa todos los proyectos con interfaz de usuario y se organiza **por áreas de la compañía**. Los dos proyectos principales son `website` (presencia web pública) y `backoffice` (app interna de administración, pensada para alojar varias soluciones en un mismo proyecto: autenticación, gestión de personas, gestión de operaciones y comunicación interna). Cada subcarpeta incluye su documentación técnica y funcional.
- **`services/README.md`**: cada subcarpeta corresponde a **un servicio concreto** — sus ejemplos son `admin-api` y `data-processor-worker` — con su documentación técnica y funcional.

Ambos README piden **registrar cada aplicación o servicio nuevo** en el README de la carpeta, indicando su objetivo, la tecnología usada y cómo ejecutarlo.

Ejemplos para Nexova: `services/selection-api`, `services/support-api` (los dashboards y portales internos se agrupan dentro de `uis/backoffice`), `agents/support-agent`, `mcps/hubspot-mcp`, `packages/shared-types`, `workflows/sales-follow-up`, `scripts/seed-fake-candidates`.

### Límites que no se cruzan

- **`.agents/` no es `agents/` ni `skills/`.** En `.agents/` se configura la *herramienta de desarrollo* (cómo trabaja el agente de código aquí). En `agents/` y `skills/` vive el *producto*: los agentes y capacidades que usará Nexova. Nunca se mezclan.
- **`memory-bank/` no duplica `docs/`.** El banco de memoria es el contexto corto y activo que el agente lee al empezar; `docs/` son guías detalladas para personas.
- **`packages/` frente a `shared/`.** Código que se importa o versiona → `packages/`. Archivos que se copian o consultan (plantillas, esquemas) → `shared/`.
- **`internal/` frente a `scripts/`.** Herramienta con dependencias y tests → `internal/`. Script suelto → `scripts/`.
- **`workflows/` frente a `services/`.** Orquestación entre sistemas y jobs programados → `workflows/`. Lógica de negocio expuesta como API → `services/`.
- **Ningún árbol paralelo.** No se crean `apps/`, `frontend/`, `api/` ni similares. Los frontends van en `uis/` y los backends en `services/`.
  > Los README de `packages/` y `shared/` mencionan una carpeta `apps/` que no existe en la plantilla: se interpreta como `uis/` + `services/`.
- **Cada componente nuevo se registra** en el README de su carpeta padre. Toda carpeta de primer nivel nueva requiere un ADR en `decisions.md` y la aprobación del tech lead.

### Flujo de datos previsto

```text
Sistemas legacy (HubSpot, Zendesk, ATS propio, Google Workspace)
        │
        ├──► mcps/*  ──────────────►  agents/* (chatbots, RAG, scoring)
        │                                    │
        ├──► workflows/* (sincronización, secuencias, alertas)
        │                                    │
        ▼                                    ▼
   services/*  ──►  datos / KPIs  ──►  uis/backoffice (dashboards)
                                       uis/website (web pública)
```

## 4. Integraciones previstas

Los productos tendrán que leer o escribir en los sistemas legacy durante la transición: **HubSpot** (pipeline, secuencias y alertas), **Zendesk** (tickets, chatbot y sentimiento), **el ATS propio** (candidatos, scoring y RAG) y **Google Workspace** (formularios, hojas y email).

Los conectores se implementan como servidores MCP en `mcps/` o como integraciones dentro de `services/`.

**Los secretos nunca van al repositorio.** Se gestionan con variables de entorno y se documentan en un `.env.example` sin valores reales.

## 5. Restricciones técnicas

- **Datos personales:** candidatos, empleados y clientes de la UE y EE. UU. Los seeds, fixtures y tests usan siempre datos ficticios.
- **Explicabilidad:** los servicios de scoring devuelven la puntuación **junto con los factores que la justifican**.
- **SLA de soporte:** 24 h. Las automatizaciones de soporte registran sus tiempos y escalan a un humano.
- **Observabilidad desde el inicio:** todo servicio nuevo emite logs estructurados, porque es una carencia explícita de la empresa.
- **Accesibilidad y rendimiento** obligatorios en `uis/website`.
- **Una subcarpeta, un componente**, con su propio `README.md`, en `services/`, `agents/`, `mcps/` y `packages/`. Cada componente nuevo se **registra además en el README de su carpeta padre**, con su objetivo, la tecnología usada y cómo ejecutarlo, como piden los README de la plantilla.

## 6. Entorno de desarrollo y despliegue

- **Entorno:** GitHub Codespaces (hay `.devcontainer/`) o clon local.
- **Flujo Git:** ramas `feature/<nombre>` creadas desde `main` actualizado y PR hacia `main` del fork. El detalle está en `AGENTS.md`.
- **Despliegue:** hoy manual en la empresa; el objetivo es automatizarlo en `infra/`.
- **Comandos** de instalación, lint, test y build: **por definir** hasta que exista la primera app. Se documentarán aquí y en `AGENTS.md`.

## 7. Documentos relacionados

- `memory-bank/projectbrief.md` — negocio, problema y objetivos.
- `memory-bank/decisions.md` — el porqué de cada decisión (ADR).
- `memory-bank/progress.md` — estado actual y próximos pasos.
- `AGENTS.md` y `.agents/rules/` — flujo de trabajo obligatorio del agente de código.
