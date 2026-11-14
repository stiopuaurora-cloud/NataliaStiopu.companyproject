# Skills del agente de código

> Capacidades reutilizables para trabajar de forma consistente en este repositorio. Formato: `<skill-name>/SKILL.md`.
> No confundir con `skills/`, en la raíz: aquí van skills de la **herramienta de desarrollo**; en `skills/` va **código de producto** (capacidades de los agentes de Nexova).

## Cómo se escribe una skill

Cada `SKILL.md` incluye:

1. **Objetivo** — una sola frase.
2. **Entradas** — qué debe aportar el humano o el agente.
3. **Pasos** — la secuencia de ejecución.
4. **Criterios de aceptación** — casillas verificables, sin juicio subjetivo.
5. **Fuera de alcance** — lo que esta skill no hace.

## Skills de este repositorio

| Skill | Cuándo se usa | Qué produce |
|---|---|---|
| [`delivery-checklist/SKILL.md`](./delivery-checklist/SKILL.md) | Antes de empujar una rama y abrir un PR | La checklist de `AGENTS.md` cumplimentada y la confirmación de que la rama está lista |

## Skills previstas

Añadir solo cuando exista la primera app, para poder escribir criterios verificables:

- `update-memory-bank` — actualizar el banco de memoria tras cerrar una feature.
- `add-backoffice-route` — añadir una ruta nueva siguiendo las convenciones de `uis/backoffice`.
- `add-service-endpoint` — añadir un endpoint a un servicio de `services/`.

Al añadir una skill: créala en su carpeta y **añádela a esta tabla**.
