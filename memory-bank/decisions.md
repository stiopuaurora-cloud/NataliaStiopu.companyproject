# Decisions Log (ADR ligero)

> Registro de decisiones de arquitectura y de proceso: el *porqué* de la configuración del proyecto.
> **Las decisiones no se borran.** Si una cambia, se marca como `Reemplazada por ADR-XXX` y se añade una nueva.
> Formato: contexto → decisión → consecuencias.

---

## ADR-001 — Trabajar sobre el fork del monorepo de plantilla

- **Fecha:** 2026-11-14
- **Estado:** Aceptada
- **Contexto:** El proyecto parte de la plantilla `ai-engineering-company-project-monorepo` y todo el trabajo futuro (UIs, APIs y agentes) debe convivir en un mismo espacio.
- **Decisión:** Todo el desarrollo se hace en el fork del monorepo. No se crean repositorios nuevos.
- **Consecuencias:** Una sola fuente de verdad. Las ramas `feature/*` hacen PR hacia `main` del fork.

## ADR-002 — Banco de memoria en `memory-bank/`

- **Fecha:** 2026-11-14
- **Estado:** Aceptada
- **Contexto:** Sin contexto persistente, cada sesión del agente empieza de cero y repite los mismos errores.
- **Decisión:** El contexto vive en `memory-bank/`. Los tres archivos requeridos — `projectbrief.md`, `techContext.md` y `progress.md` — son autosuficientes: un agente que solo lea esos tres tiene lo necesario para trabajar. `product-context.md` y `decisions.md` los amplían sin repetirlos.
- **Consecuencias:** Hay que actualizarlos en cada PR que cambie decisiones, arquitectura o estado. Ver "Cómo se mantiene el banco de memoria" al final.

## ADR-003 — Separar `.agents/` de `agents/` y `skills/`

- **Fecha:** 2026-11-14
- **Estado:** Aceptada
- **Contexto:** La plantilla tiene carpetas `agents/` y `skills/` para código de producto, y es fácil confundirlas con la configuración del agente de código. Además, el README de `skills/` describe skills que "ayudan a trabajar en el monorepo", lo que agrava la ambigüedad.
- **Decisión:** En `.agents/` va solo la configuración de la herramienta de desarrollo: reglas en `.agents/rules/<rule-name>.md` y skills en `.agents/skills/<skill>/SKILL.md`. Las carpetas `agents/` y `skills/` quedan reservadas a producto: agentes que usará Nexova y sus capacidades.
- **Consecuencias:** Nunca se mezclan, aunque el nombre se parezca. Una regla para el agente de código jamás va en `agents/`, y el código de un chatbot jamás va en `.agents/`.

## ADR-004 — Capa de aplicación en las rutas de la plantilla

- **Fecha:** 2026-11-14
- **Estado:** Aceptada
- **Contexto:** El tech lead prohíbe crear un árbol paralelo.
- **Decisión:** La web pública va en `uis/website`, la app interna en `uis/backoffice` y las APIs en `services/`. Antes de crear nada hay que leer el README de cada carpeta.
- **Consecuencias:** Cualquier carpeta de primer nivel nueva requiere un ADR y la aprobación del tech lead.

## ADR-005 — `BRIEFING.md` como fuente de verdad del negocio

- **Fecha:** 2026-11-14
- **Estado:** Aceptada
- **Contexto:** El banco de memoria y las reglas deben describir a Nexova, no a una empresa genérica.
- **Decisión:** `BRIEFING.md`, en la raíz, contiene el briefing oficial de Nexova. Si entra en conflicto con el banco de memoria, manda el briefing.
- **Consecuencias:** Las contradicciones del propio briefing se registran como preguntas abiertas en `progress.md` en lugar de resolverse por cuenta propia.

## ADR-006 — Privacidad y explicabilidad como restricciones de diseño

- **Fecha:** 2026-11-14
- **Estado:** Aceptada
- **Contexto:** Nexova maneja datos personales de candidatos, empleados y clientes de la UE y EE. UU., y su ventaja competitiva es el scoring y el matching de personas.
- **Decisión:** Nunca se usan datos personales reales en el repositorio. Todo scoring o ranking devuelve los factores que lo explican.
- **Consecuencias:** Los seeds y tests usan datos ficticios. Las APIs de scoring incluyen el campo de explicación.

---

## Plantilla para nuevas decisiones

```markdown
## ADR-XXX — Título
- **Fecha:** AAAA-MM-DD
- **Estado:** Propuesta | Aceptada | Reemplazada por ADR-YYY
- **Contexto:** ¿Qué problema o situación motiva la decisión?
- **Decisión:** ¿Qué se decide?
- **Consecuencias:** ¿Qué implica (positivo y negativo)?
```

## Cómo se mantiene el banco de memoria

| Archivo | Cuándo se actualiza |
|---|---|
| `projectbrief.md` | Casi nunca. Solo si el briefing cambia o aparece información nueva del negocio |
| `techContext.md` | Al elegir una tecnología, al cambiar la arquitectura o al añadir una integración |
| `progress.md` | En **cada** tarea o PR: qué está hecho, en curso y qué viene después |
| `product-context.md` | Al añadir, quitar o priorizar productos del plan |
| `decisions.md` | Cada vez que se toma una decisión con consecuencias duraderas |

## Documentos relacionados

- `memory-bank/projectbrief.md` — negocio y objetivos.
- `memory-bank/techContext.md` — stack, arquitectura y restricciones.
- `memory-bank/progress.md` — estado actual.
- `memory-bank/product-context.md` — plan de productos.
