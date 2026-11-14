# AGENTS.md — Protocolo del agente de código

> Este archivo define **cómo opera cualquier agente de código** (Cursor, Claude Code, Windsurf, Copilot…) en este repositorio. Es de obligado cumplimiento: ningún agente escribe código sin pasar por el proceso de entrega.
> Proyecto: **Nexova Solutions** · Monorepo: fork de `ai-engineering-company-project-monorepo`
> **Última actualización:** 2026-11-14

---

## 0. Regla de oro

**Nada se commitea sin pasar por la sección 2.** Si un paso no se puede ejecutar (por ejemplo, aún no hay tests porque no existe la primera app), se dice explícitamente en el mensaje del commit o en el PR, en lugar de omitirlo en silencio.

---

## 1. Inicio de sesión — lectura obligatoria

**Antes de editar, crear o borrar cualquier archivo**, el agente lee estos archivos, en este orden:

| # | Archivo | Para qué |
|---|---|---|
| 1 | `BRIEFING.md` | Fuente de verdad del negocio. Si algo entra en conflicto con el banco de memoria, manda este archivo |
| 2 | `memory-bank/projectbrief.md` | Negocio, problema, objetivos, usuarios, alcance y restricciones de negocio |
| 3 | `memory-bank/techContext.md` | Stack, mapa de carpetas, reglas de arquitectura y restricciones técnicas |
| 4 | `memory-bank/progress.md` | Qué está implementado hoy, qué está en curso y qué viene después |
| 5 | `memory-bank/product-context.md` | Plan de productos por área y en qué carpeta vivirá cada uno |
| 6 | `memory-bank/decisions.md` | Decisiones ya tomadas (ADR) y cuándo se actualiza cada archivo de la memoria |
| 7 | `.agents/rules/README.md` | Índice de reglas de desarrollo y cuándo aplica cada una |
| 8 | `AGENTS.md` | Este protocolo |
| 9 | El `README.md` de la carpeta donde va a trabajar | Qué debe ir en esa carpeta y qué no |
| 10 | `memory-bank/techContext.md` §3 | Los límites que no se cruzan (`.agents/` vs `agents/`, `packages/` vs `shared/`…) |
| 11 | `.agents/skills/delivery-checklist/SKILL.md` | Checklist de entrega a ejecutar antes del PR |

**Si el agente no puede leer alguno de estos archivos, no empieza a editar.** Lo dice y pide indicaciones.

Antes de escribir la primera línea, comprueba además:

- En qué rama está (`git branch --show-current`). **No se trabaja nunca sobre `main`.** Si estás en `main`, crea una rama `feature/<nombre-descriptivo>`.
- Que la rama parte de `main` actualizado (`git fetch origin && git merge origin/main`).

---

## 2. Flujo previo a cada commit (orden obligatorio)

Los cinco pasos se ejecutan **en este orden**. Si un paso falla, se corrige antes de seguir con el siguiente.

1. **Formatear y pasar el linter.** Ejecuta el formatter y el linter del proyecto o de la carpeta tocada. Si aún no existe la configuración (no hay primera app), decláralo y continúa al paso 2.
   - `npm run format` / `npm run lint` en el paquete correspondiente, cuando existan.
2. **Pasar los tests o el typecheck.** Ejecuta la suite de la carpeta tocada. Si no existen tests para ese código, dilo explícitamente en el commit en lugar de inventar que pasan.
   - `npm test` / `npm run typecheck` en el paquete correspondiente, cuando existan.
3. **Actualizar `memory-bank/progress.md` si cambió el comportamiento.** Si el commit añade, cambia o elimina funcionalidad, actualiza las secciones *Implementado* y *En curso* y la fecha. Si además se tomó una decisión duradera, añade un ADR en `memory-bank/decisions.md`. Si el commit es solo documentación o un cambio de formato, escribe `sin cambio de comportamiento` en el cuerpo del commit.
4. **Revisar el alcance del diff.** Ejecuta `git diff --staged --stat` y comprueba que **todos** los archivos tocados pertenecen a la tarea. Ningún archivo de las áreas protegidas de la sección 3, ninguna carpeta nueva de primer nivel, ningún `console.log` ni código comentado de depuración, ninguna credencial. Si algo se coló, sácalo del stage.
5. **Escribir el mensaje con Conventional Commits.** `tipo(alcance): descripción en imperativo`. Tipos permitidos: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `ci`, `infra`. En el cuerpo, indica qué archivos de la memoria actualizaste.

### Después del commit, antes del PR

- Ejecuta la skill `.agents/skills/delivery-checklist/SKILL.md` y marca cada casilla.
- Empuja la rama y abre el PR hacia `main` **del fork**, nunca hacia el repositorio de la plantilla.
- En la descripción del PR, incluye un enlace directo a este `AGENTS.md`.

---

## 3. Áreas protegidas

Estas rutas **no se modifican sin confirmación humana explícita**. Si una tarea parece exigirlo, el agente se detiene, explica por qué y espera respuesta.

| Ruta | Motivo |
|---|---|
| `.github/workflows/`, cualquier `*.yml` de CI, `.devcontainer/` | Cambiar la integración continua o el contenedor afecta a todo el equipo |
| `package-lock.json`, `pnpm-lock.yaml`, `yarn.lock`, `poetry.lock`, `requirements*.lock` | Archivos generados. Solo se regeneran con el gestor de paquetes, nunca a mano |
| `BRIEFING.md` | Fuente de verdad del negocio. Solo cambia si cambia el briefing oficial |
| `memory-bank/projectbrief.md` | Se modifica solo cuando cambia el briefing, no como efecto secundario de una tarea |
| `*.env`, `*.env.*` salvo `.env.example`, y cualquier archivo de credenciales | Seguridad. Nunca se suben secretos |
| Carpetas de otros componentes o de otros compañeros (`services/<otro>`, `uis/<otra>`, `agents/<otro>`…) | Cada componente se toca desde su propia rama y su propio PR |
| `README.md` de las carpetas de primer nivel de la plantilla | Contienen las instrucciones de la plantilla; se editan solo para registrar un componente nuevo, y lo indica el propio README |
| `infra/` | Cambiar infraestructura o despliegue afecta a producción |

**Siempre protegidas, sin excepción:** crear carpetas nuevas de primer nivel y **crear un árbol paralelo** (`apps/`, `frontend/`, `api/`, `backend/`…). Los frontends van en `uis/` y los backends en `services/`. Cualquier carpeta nueva de primer nivel requiere un ADR y aprobación del tech lead.

---

## 4. Convenciones que el agente debe respetar

- **Dónde vive el código nuevo:** web pública en `uis/website`, app interna en `uis/backoffice`, APIs y workers en `services/<servicio>`, agentes de producto en `agents/<agente>`, servidores MCP en `mcps/<servidor>-mcp`, automatizaciones en `workflows/`, paquetes compartidos en `packages/<paquete>`, recursos sin empaquetar en `shared/`. El detalle está en `memory-bank/techContext.md` §3.
- **Un componente, una subcarpeta, un README.** Cada servicio, agente, servidor MCP o paquete nuevo se documenta en su propio `README.md` y se registra en el README de su carpeta padre.
- **Ramas:** `feature/<nombre-descriptivo>`, en minúsculas y separadas por guiones. Nunca se trabaja sobre `main`.
- **Commits:** Conventional Commits, en imperativo y con alcance.
- **Datos:** nunca datos personales reales. Seeds, fixtures y tests usan datos ficticios.
- **Secretos:** solo variables de entorno, documentadas en un `.env.example` sin valores reales.
- **Logs:** todo servicio nuevo emite logs estructurados (es una carencia explícita de Nexova).
- **Explicabilidad:** cualquier código de scoring o ranking devuelve la puntuación **junto con los factores que la justifican**.

---

## 5. Cuándo detenerse y preguntar

El agente **para y pregunta** (no decide por su cuenta) cuando:

- La tarea requiere tocar un área protegida de la sección 3.
- La tarea exige crear una carpeta de primer nivel o un árbol paralelo.
- Hay que elegir una tecnología que `memory-bank/techContext.md` marca como **por definir** (base de datos, vector store, proveedor de LLM, CI/CD…).
- El briefing se contradice a sí mismo o entra en conflicto con el banco de memoria. Ejemplo abierto: el responsable de Ventas (ver `memory-bank/progress.md`).
- El cambio afecta a datos personales o a una decisión de negocio.
- La tarea implica tocar archivos de otro componente o de otro compañero.

Al preguntar, el agente indica: qué archivo o carpeta quiere tocar, por qué lo necesita y qué alternativas ve.

---

## 6. Errores que el agente no debe cometer

1. Escribir sobre `main` o commitear sin pasar por la sección 2.
2. Crear un árbol paralelo o una carpeta de primer nivel sin ADR.
3. Mezclar configuración del agente de código con código de producto (`.agents/` vs `agents/` y `skills/`).
4. Inventar stack, nombres de archivo o datos que no estén en el briefing o en el banco de memoria.
5. Usar datos personales reales en ejemplos, seeds o tests.
6. Subir secretos, `.env` o lockfiles editados a mano.
7. Actualizar `progress.md` con trabajo que no está realmente terminado.

---

## Documentos relacionados

- `memory-bank/projectbrief.md` — negocio, objetivos y restricciones de negocio.
- `memory-bank/techContext.md` — stack, arquitectura y restricciones técnicas.
- `memory-bank/progress.md` — estado actual del desarrollo.
- `memory-bank/decisions.md` — decisiones tomadas (ADR).
- `.agents/rules/README.md` — índice de reglas de desarrollo.
- `.agents/skills/delivery-checklist/SKILL.md` — checklist obligatoria antes del PR.
