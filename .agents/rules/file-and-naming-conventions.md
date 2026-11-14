---
scope: always-on
---

# Convenciones de archivos, nombres y commits

Aplica a toda sesión, sin excepción. Es la parte que hace que el repositorio se pueda leer y navegar.

## 1. Nombres de archivos y carpetas

- Todo en **minúsculas**, con **guiones** para separar palabras: `support-agent`, `cv-scoring-worker`, `projectbrief.md`.
- **Se permiten mayúsculas solo** en los archivos que la plantilla ya nombra así: `README.md`, `README.es.md`, `AGENTS.md`, `BRIEFING.md`, `SKILL.md`, `SKILL.es.md`.
- **Nombres de archivo del banco de memoria: no se renombran.** `projectbrief.md`, `techContext.md` y `progress.md` son los nombres requeridos del proyecto. `product-context.md` y `decisions.md` siguen el mismo patrón.
- Un componente por carpeta. Los nombres de carpeta se escriben en singular cuando designan un componente (`services/selection-api`, `agents/support-agent`) y en plural solo cuando son carpetas contenedoras de la plantilla (`services/`, `agents/`, `skills/`, `mcps/`, `workflows/`, `packages/`).
- Sin espacios, sin acentos y sin `ñ` en nombres de archivo o carpeta. Se permiten en el contenido y en el copy visible.

## 2. Ramas

- Formato: `feature/<nombre-descriptivo>`, en minúsculas y con guiones.
- Una rama por tarea, creada **desde `main` actualizado**:
  ```bash
  git checkout main && git pull origin main
  git checkout -b feature/mi-tarea
  ```
- **Nunca se trabaja sobre `main`.** Ni directa, ni con un commit "pequeño".
- Nombres de rama del hito actual: `feature/agent-memory-bank`.

## 3. Commits

- **Conventional Commits**: `tipo(alcance): descripción en imperativo`.
- Tipos permitidos: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `ci`, `infra`.
- El alcance es la carpeta o el componente afectado: `feat(selection-api): añade scoring de CVs`.
- Un commit = un cambio coherente. Si tocas dos cosas sin relación, son dos commits.
- Si el commit no cambia comportamiento, indícalo en el cuerpo (ver `AGENTS.md` §2, paso 3).

## 4. Imports y referencias

- No se inventan alias de import nuevos sin un ADR. Hasta que exista la primera app con su `tsconfig`, se usa **rutas relativas dentro del componente**.
- Un componente **no importa código de otro componente directamente**: si hay que compartir, el código sube a `packages/<paquete>` y se importa desde ahí.
- Las rutas de los documentos del repositorio se escriben desde la raíz del monorepo, no desde el archivo que las cita: `memory-bank/progress.md`, no `../progress.md`.

## 5. Estilo

- El formatter y el linter son la autoridad, no la preferencia del agente. Se ejecutan antes de cada commit (`AGENTS.md` §2, paso 1).
- Sin `console.log` ni código comentado de depuración en el stage.
- Los comentarios explican el *porqué*, no repiten lo que ya dice el código.
- Todo el texto visible para el usuario final va en español, coherente con el copy de Nexova.

## Cómo verificar esta regla

- [ ] `git branch --show-current` no devuelve `main` en ningún momento del trabajo.
- [ ] `git log --oneline -5` muestra todos los mensajes con formato `tipo(alcance): descripción`.
- [ ] `git diff --staged --stat` no muestra archivos con espacios, acentos ni mayúsculas fuera de los permitidos.
- [ ] Ninguna carpeta ni archivo nuevo aparece fuera de las rutas que mapea `memory-bank/techContext.md` §3.

## Documentos relacionados

- `AGENTS.md` §2 y §4 — flujo de commits y convenciones generales.
- `.agents/rules/app-layer-placement.md` — dónde vive cada tipo de código.
