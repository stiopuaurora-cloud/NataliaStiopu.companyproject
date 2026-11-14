---
scope: always-on
---

# Datos, secretos, explicabilidad y observabilidad

Aplica a toda sesión. Estas reglas vienen de restricciones reales de Nexova (`BRIEFING.md`) y de decisiones ya tomadas (`memory-bank/decisions.md`, ADR-006).

## 1. Datos personales

Nexova maneja datos de **candidatos, empleados y clientes** con sedes en la UE (Valencia) y EE. UU. (Miami).

- **Nunca se usan datos personales reales** en el repositorio: ni en ejemplos, ni en seeds, ni en fixtures, ni en tests, ni en capturas de pantalla.
- Los datos de prueba se generan con nombres, emails y teléfonos **ficticios** y claramente marcados como tales (por ejemplo, `candidato.demo@example.com`).
- Los ficheros de datos de ejemplo van en `shared/` o en el `scripts/` que los genera, nunca sueltos en la raíz.
- Si una tarea parece necesitar datos reales para reproducir un problema, el agente **para y pregunta**.

## 2. Secretos y credenciales

- **Ningún secreto entra en el repositorio**: ni tokens, ni API keys, ni contraseñas, ni cadenas de conexión.
- Se gestionan con **variables de entorno**. El repositorio solo contiene un `.env.example` con los nombres de las variables y valores falsos.
- **Nunca** se imprime un secreto en un log, en un mensaje de error ni en la salida de un test.
- Las integraciones con HubSpot, Zendesk, el ATS propio y Google Workspace se autentican por variables de entorno, nunca con credenciales embebidas en el código.
- Si el agente detecta un secreto ya commiteado, lo dice de inmediato y **no intenta borrarlo del historial por su cuenta**: eso requiere intervención humana.

## 3. Explicabilidad del scoring

La ventaja competitiva de Nexova es el scoring y el matching de personas, así que ningún resultado puede ser una caja negra (ADR-006).

- Toda función que puntúe, ordene o clasifique personas **devuelve, además de la puntuación, los factores que la justifican** (por ejemplo, un array de `{ factor, peso, valor }`).
- Nada de rankings "porque sí": si el modelo no puede explicar un factor, ese factor no puntúa.
- La persona que decide (consultor, supervisor, manager) siempre puede ver el motivo y anular el resultado.

## 4. Logs y observabilidad

- **Todo servicio nuevo emite logs estructurados** (JSON o clave-valor), no texto libre concatenado. Es una carencia explícita de Nexova: no hay telemetría ni logs centralizados.
- Cada log incluye, como mínimo: nivel, servicio, marca de tiempo y un identificador de correlación de la petición.
- **Nunca se registran datos personales** en los logs: se registran identificadores, no nombres, emails ni teléfonos.
- Las automatizaciones de soporte **registran sus tiempos de resolución** y dejan traza de cuándo escalan a un humano, porque el SLA comprometido es de 24 h frente a las 48 h actuales.

## Cómo verificar esta regla

- [ ] `git diff --staged --name-only | grep -Ei '\.env($|\.)'` devuelve como mucho un `.env.example`.
- [ ] `grep -RniE '(api[_-]?key|secret|password|token)\s*[:=]\s*["'"'"'][A-Za-z0-9_-]{12,}' <archivos cambiados>` no devuelve nada.
- [ ] Los datos de ejemplo tienen dominios `example.com` o nombres evidentemente ficticios.
- [ ] Toda función de scoring devuelve el campo de explicación junto a la puntuación.
- [ ] Los servicios nuevos emiten logs estructurados y no incluyen campos de datos personales.

## Documentos relacionados

- `memory-bank/projectbrief.md` §6 — restricciones de negocio.
- `memory-bank/techContext.md` §5 — restricciones técnicas.
- `memory-bank/decisions.md` — ADR-006.
- `AGENTS.md` §3 — áreas protegidas, incluidos los archivos `.env`.
