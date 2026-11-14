# Catálogo de productos — Plan a futuro

> Este archivo es el **plan**: qué vamos a construir para cada área, derivado de los problemas descritos en `projectbrief.md`.
> Para saber qué está hecho de verdad, ver `memory-bank/progress.md`. Un producto de esta lista no está construido por aparecer aquí.
> **Última actualización:** 2026-11-14

## Principios de producto

1. **Explicabilidad**: ningún ranking de personas sin el motivo visible.
2. **Humano en el bucle**: la IA propone y la persona decide en lo crítico (contratación, escalados).
3. **Datos en tiempo real** frente a informes manuales.
4. **Accesibilidad y rendimiento** en todo lo público, porque la web actual falla en ambos.
5. **B2B primero**: interfaces para profesionales con flujos de trabajo concretos.

## Productos por área

### 🔍 Operaciones de Selección (núcleo del negocio)

- Pipeline de selección con **scoring y ranking automático de CVs**, explicable
- **RAG sobre la base de candidatos**, con búsquedas en lenguaje natural (p. ej. "perfiles con ventas B2B e inglés C1")
- Portal de candidatos con estado en tiempo real
- Agente de comunicación con emails automáticos de seguimiento

### 📞 Soporte al Cliente

- Chatbot RAG de primera línea, con el objetivo de **resolver el 40 % de las consultas** y escalar el resto
- Base de conocimiento centralizada con búsqueda semántica
- Dashboard de soporte en tiempo real: carga, backlog y cumplimiento del SLA
- Análisis de sentimiento de tickets para detectar riesgo de escalado

### 🎓 Formación Corporativa

- Catálogo online con búsqueda e inscripción
- Recomendador de programas según el perfil del cliente
- Portal del alumno con seguimiento de progreso
- Chatbot de asesoramiento con planes formativos personalizados

### 💼 Ventas y Desarrollo de Negocio

- Dashboard del pipeline
- Secuencias automáticas de prospección: primer contacto → seguimiento → cierre o descarte
- Alertas de conversaciones sin actividad
- Agente que sugiere el ángulo de propuesta para cada prospecto

### 🌐 Marketing y Comunicación

- **Rediseño de la web corporativa** con SEO/GEO, marcado schema.org y accesibilidad
- Pipeline de contenido asistido por IA
- Dashboard de métricas: visitas, fuentes de tráfico y conversión

### 🧑‍🤝‍🧑 Recursos Humanos (interno)

- Portal interno de RRHH: vacaciones, ausencias y consultas
- Onboarding automatizado con checklists progresivos
- Dashboard de KPIs: rotación, absentismo y tiempo de cobertura de vacantes
- Agente de preguntas sobre políticas y beneficios

### 💻 Tecnología e Infraestructura

- Telemetría y logging centralizados
- Pipeline de datos que alimenta todos los dashboards
- Monitorización con alertas automáticas
- Agente de ingeniería que responde consultando la documentación técnica

### 📊 Dirección Ejecutiva

- Dashboard ejecutivo unificado con KPIs en tiempo real
- Informe semanal generado automáticamente
- Alertas por umbrales estratégicos
- Asistente de IA en lenguaje natural

## Dónde vivirá cada producto

| Tipo | Ubicación |
|---|---|
| Web pública | `uis/website` |
| Apps internas (dashboards, portales, herramientas de consultores) | `uis/backoffice` |
| APIs y workers | `services/<servicio>` |
| Agentes de IA de producto | `agents/<agente>` |
| Capacidades reutilizables de esos agentes | `skills/<skill>` |
| Conectores con HubSpot, Zendesk o el ATS | `mcps/<servidor>-mcp` |
| Secuencias, alertas e informes automáticos | `workflows/` |
| Tipos, UI y SDKs compartidos | `packages/<paquete>` |

## Documentos relacionados

- `memory-bank/projectbrief.md` — problema y objetivos que justifican estos productos.
- `memory-bank/techContext.md` — arquitectura y restricciones técnicas.
- `memory-bank/progress.md` — estado real del desarrollo.
