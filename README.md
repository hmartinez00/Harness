# Harness

Repositorio de recursos para configurar y operar un harness de trabajo asistido
por IA sobre proyectos de software. La base actual parte de la experiencia con
un proyecto legacy en Laravel 12 y está destinada a evolucionar para permitir
la configuración de proyectos existentes y nuevos mediante un flujo completo,
reutilizable y automatizado.

## Estado actual

Este repositorio contiene principalmente instrucciones en Markdown: prompts de
exploración, agentes, comandos, una skill especializada y una iniciativa de
ejemplo. **Todavía no es un instalador ni un harness universal listo para
aplicarse sin adaptación.**

Parte de los recursos conserva rutas, convenciones, nombres y supuestos de un
proyecto concreto. Deben tratarse como material de partida: antes de usarlos en
otro repositorio, hay que comprobar sus instrucciones contra la estructura,
herramientas, stack y políticas reales del proyecto destino. No se debe copiar
como hecho o regla general aquello que solo sea un ejemplo o una decisión local.

El objetivo de evolución es conectar estos recursos en un proceso que descubra
el proyecto, proponga y valide su configuración y deje un harness coherente
tanto para proyectos legacy como para proyectos nuevos. Ese proceso completo
aún no está implementado aquí.

## Contenido

Los recursos se encuentran bajo [`laravel_harness/`](./laravel_harness/):

| Ruta | Propósito |
| --- | --- |
| [`explore/`](./laravel_harness/explore/) | Prompts para explorar un proyecto y preparar recursos de harness. Incluye una secuencia numerada para auditoría legacy, propuesta de gobernanza, baseline, `AGENTS.md` y Constitution, además de prompts sobre autoconfiguración y especificación portable. |
| [`agents/`](./laravel_harness/agents/) | Instrucciones para agentes de implementación y auditoría documental. Los documentos actuales contienen contexto y reglas de un proyecto concreto. |
| [`commands/`](./laravel_harness/commands/) | Flujos de trabajo para especificar iniciativas y conectar vistas a datos. Revisar su alcance antes de reutilizarlos; no son comandos ejecutables por sí solos. |
| [`skills/`](./laravel_harness/skills/) | Skill de diseño de CRUDs Laravel Blade con TailAdmin, enfocada en formularios, autorización, contratos y experiencia de usuario. |
| [`specs/gestion-documental/`](./laravel_harness/specs/gestion-documental/) | Ejemplo de una iniciativa documentada mediante `spec.md`, `plan.md` y `tasks.md`; refleja decisiones y contexto particulares, no una plantilla universal ya validada. |
| [`arnes-configuracion-agentes-2026-09.md`](./laravel_harness/arnes-configuracion-agentes-2026-09.md) | Registro histórico de una configuración de agentes y herramientas en un proyecto. No es la fuente de estado actual de un proyecto destino. |
| [`external.txt`](./laravel_harness/external.txt) | Referencias externas y notas de instalación recopiladas durante la exploración. |

## Flujo de referencia para proyectos legacy

Los prompts numerados presentan una secuencia inicial para preparar el trabajo
con un sistema existente:

1. Auditar el proyecto y recopilar evidencia sin modificar el código.
2. Proponer por separado las reglas operativas, los principios de gobierno, la
   documentación arquitectónica y la deuda técnica.
3. Consolidar un baseline que distinga hechos verificados, inferencias,
   decisiones y validaciones pendientes.
4. Preparar instrucciones operativas para agentes (`AGENTS.md`).
5. Preparar los principios de gobierno del proyecto (Constitution).

Después, los recursos de especificación e implementación describen cómo
trabajar sobre iniciativas delimitadas. El orden y el grado de automatización
deben verificarse y adaptarse al harness elegido y al proyecto destino. Una
propuesta o plan no autoriza por sí sola la implementación.

## Uso recomendado

1. Selecciona los recursos pertinentes; no copies todo el directorio a ciegas.
2. Inspecciona primero el repositorio destino y contrasta cada instrucción con
   su stack, estructura, dependencias y herramientas.
3. Mantén separados los hechos del proyecto, las inferencias, las propuestas y
   las decisiones que requieren aprobación.
4. Adapta o elimina referencias específicas al proyecto de origen antes de
   convertirlas en instrucciones para otro proyecto.
5. Verifica los archivos y comportamientos que el flujo genere; no des por
   hecho que las herramientas externas, los comandos o las rutas mencionadas
   están instalados o configurados.

La skill de CRUD se limita a interfaces administrativas Laravel Blade con
TailAdmin. No implica que todos los proyectos destino usen Laravel, Blade,
TailAdmin o Pest.

## Requisitos y validación

El repositorio no contiene una aplicación Laravel, un instalador ni un conjunto
de pruebas ejecutables para validar la configuración de un harness completo.
Las herramientas externas mencionadas en los recursos (por ejemplo, Spec Kit,
OpenCode, MCP o el CLI de skills) deben instalarse y configurarse según sus
documentaciones y las políticas del entorno donde se vayan a usar.

Al adaptar estos recursos, valida por separado los cambios documentales, la
configuración de herramientas y cualquier cambio en el proyecto destino. No
ejecutes operaciones sobre datos, infraestructura o entornos productivos como
parte de una exploración sin autorización explícita.

## Contribuir a la evolución

Las mejoras deberían hacer que los recursos sean más portables, claros y
componibles, sin eliminar las protecciones que hacen falta en proyectos legacy.
Conviene explicitar qué recursos son genéricos, cuáles dependen del stack y
cuáles pertenecen a un proyecto de ejemplo. El objetivo es acercarse a un flujo
de autoconfiguración reproducible que diagnostique el proyecto antes de
proponer o aplicar cambios, y que conserve puntos de aprobación cuando haya
decisiones o riesgos que no puedan resolverse automáticamente.
