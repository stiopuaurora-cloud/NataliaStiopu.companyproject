# Project Brief — Nexova Solutions

> **Fuente de verdad:** `BRIEFING.md`, en la raíz del monorepo. Si este archivo y el briefing se contradicen, manda el briefing y hay que actualizar este archivo.
> **Última actualización:** 2026-11-14

## 1. Quiénes somos

- **Empresa:** Nexova Solutions
- **Sector:** consultoría de recursos humanos y selección de talento (B2B)
- **Fundación:** 2011
- **Sedes:** Valencia, España (central) y Miami, Florida (expansión)
- **Tamaño:** 120 empleados
- **Facturación:** unos 8 M USD anuales
- **CEO:** Laura Mendoza (desde la fundación)

**Líneas de negocio:**

1. **Headhunting** de mandos medios y directivos. Es la línea principal de ingresos.
2. **Outsourcing de soporte al cliente** para empresas tecnológicas, de retail y de finanzas.
3. **Formación corporativa** en habilidades blandas y liderazgo.

**Clientes:** medianas empresas de tecnología, retail y servicios financieros que externalizan parte o toda su gestión de talento. Son equipos profesionales con flujos de trabajo concretos, no consumidores anónimos.

**Quién es quién:**

| Área | Responsable |
|---|---|
| Dirección Ejecutiva | Laura Mendoza (CEO) |
| Operaciones de Selección | Javier Almeida (40 consultores) |
| Formación Corporativa | Elena Vargas (12 personas) |
| Soporte al Cliente Externalizado | Roberto Díaz (30 agentes) |
| Ventas y Desarrollo de Negocio | ⚠️ ver nota |
| Marketing y Comunicación | Carmen Ruiz |
| Recursos Humanos (interno) | Patricia Solís (4 personas) |
| Tecnología e Infraestructura | Sergio Molina (CTO, 6 personas) |

> ⚠️ **Inconsistencia del briefing:** el responsable de Ventas aparece como *Megan Clarke* en un punto y como *Marcos Ibáñez* en otro. Pendiente de confirmar; hasta entonces, usa el cargo.

## 2. El problema

Nexova hace bien su trabajo: 12 años de reputación, una red profunda de candidatos y clientes y un equipo que sabe encontrar y desarrollar talento. Lo que no tiene es **infraestructura para operar a escala**. Casi todo es manual:

- Los consultores de selección leen entre 30 y 80 CVs a mano por proceso y escriben los emails a candidatos uno a uno.
- El catálogo de formación vive en un PDF y las inscripciones en un Google Form.
- Los 30 agentes de soporte trabajan sin base de conocimiento y resuelven en 48 h lo que su SLA exige en 24 h.
- Los SDRs prospectan a mano y solo el 40 % del equipo actualiza HubSpot; se pierden deals por falta de seguimiento.
- La dirección decide con datos de hace una semana, porque cada manager dedica de 4 a 8 h al informe semanal en PDF.

Mientras tanto, plataformas automatizadas y basadas en datos compiten directamente con Nexova. La ventana para responder no es ilimitada.

## 3. Objetivos

Construir los sistemas, automatizaciones y herramientas de IA que permitan a Nexova hacer lo que ya hace bien **más rápido, a mayor escala y con mucho menos esfuerzo manual**, todo en un mismo monorepo.

**En Nexova la IA es el núcleo del negocio, no un añadido.** Son entregables centrales el scoring de CVs con rankings explicables, el matching de candidatos, el RAG sobre bases de conocimiento y los agentes de soporte de primera línea.

| Objetivo medible | Hoy | Meta |
|---|---|---|
| Resolución media de tickets de soporte | 48 h | ≤ 24 h (SLA) |
| Consultas de soporte resueltas sin humano | 0 % | 40 % |
| Visibilidad del estado de un candidato | Ninguna; el cliente llama | En tiempo real |
| Informe semanal de la CEO | Manual, 4–8 h por manager, datos de hace una semana | Automático y al día |
| Adopción del CRM por Ventas | 40 % | Por definir |

## 4. Usuarios

| Usuario | Tipo |
|---|---|
| Visitantes y clientes potenciales | Externo |
| Empresas cliente (estado de sus procesos y catálogo) | Externo |
| Candidatos | Externo |
| Alumnos de formación | Externo |
| Consultores de selección | Interno |
| Agentes y supervisores de soporte | Interno |
| Account managers y SDRs | Interno |
| RRHH y empleados | Interno |
| Marketing | Interno |
| Tecnología | Interno |
| Dirección | Interno |

## 5. Alcance de este hito

**Dentro:** `BRIEFING.md`, el banco de memoria, `AGENTS.md`, las reglas de `.agents/rules/`, al menos una skill en `.agents/skills/` con criterios de aceptación verificables, y tener documentado dónde vivirá el código de producto.

**Fuera:** crear las aplicaciones. No se crea un árbol paralelo ni carpetas nuevas de primer nivel.

## 6. Restricciones de negocio

- Se manejan **datos personales** de candidatos, empleados y clientes con sedes en la UE (Valencia) y EE. UU. (Miami). En el repositorio solo se usan datos ficticios.
- Todo scoring o ranking de personas debe ser **explicable**.
- Las automatizaciones de soporte deben **respetar los SLA** y escalar a un humano cuando no puedan resolver.
- La marca y el copy públicos deben reflejar el posicionamiento actual, no el de la web de 2019.
- El trabajo se hace **sobre el fork del monorepo de la plantilla**, no en un repositorio nuevo.

## 7. Documentos relacionados

- `memory-bank/techContext.md` — stack, arquitectura y restricciones técnicas.
- `memory-bank/progress.md` — qué está implementado, qué está en curso y qué viene después.
- `memory-bank/decisions.md` — registro de decisiones (ADR ligero).
- `AGENTS.md` y `.agents/rules/` — cómo debe trabajar el agente de código en este repositorio.
