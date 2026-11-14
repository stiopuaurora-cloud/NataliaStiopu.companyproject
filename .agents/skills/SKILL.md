# Skill: Delivery checklist antes del PR

## Objetivo

Verificar que una rama está lista para abrir un Pull Request hacia `main` del fork, ejecutando la checklist de entrega de `AGENTS.md` y dejando constancia del resultado.

## Entradas

El humano o el agente deben proporcionar:

| Entrada | Obligatoria | Descripción |
|---|---|---|
| Nombre de la rama de trabajo | Sí | Debe empezar por `feature/` |
| Alcance de la tarea | Sí | Una frase: qué cambia y por qué |
| Carpetas tocadas | Sí | Rutas del monorepo afectadas |
| Enlace del PR | No | Se rellena al final del proceso |

## Pasos

1. `git branch --show-current` — confirma que no es `main`.
2. `git fetch origin && git log --oneline origin/main..HEAD` — lista lo que la rama añade.
3. `git diff --staged --stat` y `git diff origin/main...HEAD --name-only` — revisa el alcance.
4. Ejecuta formatter y linter del paquete tocado. Si no existe configuración, anótalo como "no aplicable" con el motivo.
5. Ejecuta tests o typecheck del paquete tocado. Si no existen tests para ese código, anótalo.
6. Actualiza `memory-bank/progress.md` si hubo cambio de comportamiento; añade un ADR si hubo decisión.
7. Rellena la checklist de abajo marcando cada casilla.
8. Si todo pasa, empuja y abre el PR con el enlace a `AGENTS.md` en la descripción. Si algo falla, **no abras el PR**: reporta qué punto falló.

## Checklist de aceptación

### Rama y commits
- [ ] La rama actual no es `main`, y su nombre empieza por `feature/`.
- [ ] Todos los mensajes de commit siguen `tipo(alcance): descripción`, con un tipo permitido por `AGENTS.md` §2.
- [ ] La rama está al día con `origin/main` o se ha comprobado que no hay conflictos.

### Alcance
- [ ] `git diff origin/main...HEAD --name-only` no muestra ningún archivo fuera de las carpetas declaradas en la entrada.
- [ ] No aparece ninguna carpeta nueva de primer nivel ni rutas con `apps/`, `frontend/`, `api/` o `backend/`.
- [ ] No se ha modificado ninguna ruta de las áreas protegidas (`AGENTS.md` §3) sin confirmación humana explícita.
- [ ] Ningún lockfile aparece modificado sin haberse regenerado con el gestor de paquetes.

### Verificaciones automáticas
- [ ] El formatter y el linter han pasado, o se ha documentado que aún no existen.
- [ ] Los tests o el typecheck han pasado, o se ha documentado que no existen para ese código.
- [ ] El resultado de cada comando ejecutado está incluido en la descripción del PR.

### Banco de memoria
- [ ] `memory-bank/progress.md` está actualizado si el commit cambia comportamiento, con la fecha de cabecera correcta.
- [ ] Si hubo decisión duradera, existe un ADR nuevo en `memory-bank/decisions.md`.
- [ ] No se ha borrado ninguna decisión anterior.

### Seguridad y datos
- [ ] El diff no contiene secretos, tokens ni credenciales: solo `.env.example` en todo caso.
- [ ] Los datos de ejemplo son ficticios (`example.com` o equivalentes).
- [ ] El código de scoring devuelve su explicación junto a la puntuación, si aplica.

### Pull Request
- [ ] El PR apunta a `main` **del fork**, no al repositorio de la plantilla.
- [ ] La descripción incluye un enlace directo a `AGENTS.md`.
- [ ] La descripción resume: qué cambia, qué carpetas toca, qué verificaciones se ejecutaron y su resultado.
- [ ] Si algún paso no era aplicable, la descripción lo dice con el motivo.

## Criterios de aceptación de la skill

La skill se considera ejecutada correctamente si, y solo si:

1. Todas las casillas están marcadas como **cumplidas** o como **no aplicables con motivo escrito**. Ninguna queda en blanco.
2. Cada verificación automática lleva su comando y su resultado reales, no una afirmación de que pasó.
3. Si alguna casilla no se puede cumplir, **no se abre el PR** y se reporta el punto exacto que falla.
4. Un revisor puede comprobar cualquier casilla ejecutando el comando indicado, sin juicio subjetivo.

## Fuera de alcance

Esta skill no revisa calidad de diseño, ni decisiones de arquitectura, ni el contenido funcional del código. Eso es responsabilidad del revisor humano.

## Documentos relacionados

- `AGENTS.md` §2 y §3
- `.agents/rules/memory-bank-maintenance.md`
- `.agents/rules/data-and-secrets.md`
