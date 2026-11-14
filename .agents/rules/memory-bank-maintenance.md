---
scope: agent-requested
applies-to: memory-bank/**
---

# Mantenimiento del banco de memoria

Se carga cuando una tarea cambia el comportamiento del proyecto, cierra una decisión o toca `memory-bank/`. Su objetivo es que la memoria no quede desactualizada, porque una memoria vieja deja de ser útil en cuestión de días.

## 1. Qué archivo se toca y cuándo

| Archivo | Se actualiza cuando… | Frecuencia |
|---|---|---|
| `projectbrief.md` | Cambia el briefing oficial o aparece información nueva del negocio | Casi nunca |
| `techContext.md` | Se elige una tecnología, cambia la arquitectura o se añade una integración | Poco |
| `progress.md` | **Cada vez** que un commit cambia comportamiento | Siempre |
| `product-context.md` | Se añade, quita o prioriza un producto del plan | Cuando toque |
| `decisions.md` | Se toma una decisión con consecuencias duraderas | Cuando toque |

## 2. Qué registrar en `progress.md`

- Mueve a **Implementado** lo que está realmente terminado y quítalo de **En curso**.
- Añade o cierra **preguntas abiertas** y **problemas conocidos**.
- Actualiza la fecha de la cabecera (`Última actualización: AAAA-MM-DD`).
- No lo conviertas en una hoja de ruta de marketing: es un registro de hechos, no de intenciones.

## 3. Cómo añadir una decisión

- Añade un ADR al final de `decisions.md`, con el siguiente número libre (`ADR-00X`).
- Usa la plantilla: fecha, estado, contexto, decisión y consecuencias.
- **Nunca borres una decisión anterior.** Si cambia, márcala como `Reemplazada por ADR-XXX` y añade la nueva.
- Si la decisión afecta al alcance de las carpetas (por ejemplo, una carpeta nueva de primer nivel), actualiza también `techContext.md` §3.

## 4. Cómo cerrar una pregunta abierta

- Si se responde, elimínala de *Preguntas abiertas* **mencionando la respuesta** en la entrada correspondiente, y registra la decisión en `decisions.md` si tiene consecuencias duraderas.
- Ejemplo vivo: el responsable de Ventas aparece como *Megan Clarke* y como *Marcos Ibáñez* en el briefing. No se resuelve por cuenta propia: se pregunta y se registra la respuesta.

## 5. Regla de coherencia

Ningún archivo del banco de memoria puede citar a otro que no exista. Si renombras o eliminas un archivo, actualiza **todas** las referencias.

## Cómo verificar esta regla

- [ ] La fecha de cabecera de `memory-bank/progress.md` coincide con la fecha real del último cambio.
- [ ] Si el commit cambia comportamiento, `progress.md` aparece en `git diff --staged --name-only`.
- [ ] Si hubo decisión, existe un ADR nuevo en `decisions.md` con las cinco secciones.
- [ ] Todas las referencias entre archivos del banco de memoria apuntan a archivos existentes: `grep -rho "memory-bank/[a-zA-Z-]*\.md" memory-bank/ | sort -u` no devuelve ningún nombre inexistente.
- [ ] Ninguna decisión anterior ha sido borrada; las sustituidas están marcadas como `Reemplazada por ADR-XXX`.

## Documentos relacionados

- `memory-bank/decisions.md` — el registro y la tabla de mantenimiento.
- `AGENTS.md` §2, paso 3 — el momento exacto del flujo en que se ejecuta esto.
- `.agents/skills/delivery-checklist/SKILL.md` — comprobación final antes del PR.
