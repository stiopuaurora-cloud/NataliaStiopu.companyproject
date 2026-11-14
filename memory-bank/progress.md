# Progress — Registro vivo

> **Es un registro vivo, no una hoja de ruta de marketing.** Se actualiza al cerrar cada tarea o PR: qué está implementado, qué está en curso y qué viene después.
> Para el plan completo de los productos, ver `memory-bank/product-context.md`. Aquí está solo el estado real de hoy.
> **Última actualización:** 2026-11-14

## Implementado ✅

- [x] Fork del monorepo de plantilla `ai-engineering-company-project-monorepo`
- [x] `BRIEFING.md` en la raíz con el briefing oficial de Nexova
- [x] `memory-bank/` con el banco de memoria
- [x] `AGENTS.md` con el flujo obligatorio antes de cada commit
- [x] Reglas de desarrollo en `.agents/rules/`
- [x] Al menos una skill en `.agents/skills/<skill>/SKILL.md`, con criterios de aceptación verificables
- [x] Documentado dónde vive el código de producto, tras leer el README de cada carpeta

**Código de aplicación:** ninguno. Este hito no pide crear apps.

## En curso 🚧

Rama de trabajo: `feature/agent-memory-bank`

- [ ] Ejecutar el flujo de entrega de `AGENTS.md` antes del commit final
- [ ] Abrir el PR hacia `main` con un enlace directo a `AGENTS.md` en la descripción
- [ ] Entregar el enlace del PR en el campus

## Próximos pasos

1. **Web corporativa** en `uis/website`. Es un entregable explícito de Marketing y no depende de datos internos, así que es el mejor primer desarrollo.
2. **Base del backoffice** en `uis/backoffice`: autenticación y layout propio.
3. **Primeros servicios** en `services/`, empezando por selección (scoring de CVs), que es el núcleo del negocio.
4. **Observabilidad** transversal en cuanto exista el primer servicio: es una carencia explícita de la empresa y condiciona todo lo demás.

## Estado de las aplicaciones

| Ruta | Estado |
|---|---|
| `uis/website` | No creada |
| `uis/backoffice` | No creada |
| `services/` | Sin servicios |
| `agents/` | Sin agentes de producto |
| `mcps/` | Sin servidores |
| `workflows/` | Sin workflows |

## Preguntas abiertas ❓

- **¿Quién es el responsable de Ventas?** El briefing menciona a *Megan Clarke* y a *Marcos Ibáñez*. Pendiente de confirmar.
- **Stack técnico**: UIs, servicios, base de datos, vector store y LLM están por definir (ver `techContext.md`).
- **¿Qué proveedor de LLM y qué vector store** se usarán para el RAG de soporte y de candidatos?

## Problemas conocidos ⚠️

- Ninguno por ahora.

## Cómo actualizar este archivo

1. Mueve a *Implementado* lo que ya está terminado y quítalo de *En curso*.
2. Añade o cierra preguntas abiertas y problemas conocidos.
3. Actualiza la fecha de arriba.
4. Si hubo una decisión, regístrala también en `decisions.md`.

### Entrada sugerida

```markdown
## AAAA-MM-DD
- Hecho: …
- En curso: …
- Bloqueado por: …
- Decisión tomada: ADR-XXX
```

## Documentos relacionados

- `memory-bank/projectbrief.md` — negocio, problema y objetivos.
- `memory-bank/techContext.md` — stack y arquitectura.
- `memory-bank/product-context.md` — catálogo de productos por área y plan a futuro.
- `memory-bank/decisions.md` — decisiones tomadas (ADR).
- `AGENTS.md` — flujo de entrega obligatorio.
