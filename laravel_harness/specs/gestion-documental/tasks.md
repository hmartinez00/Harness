# Tasks: Gestion documental del repositorio

**Input**: Design documents from
`/specs/gestion-documental/`

**Prerequisites**: `spec.md` and `plan.md`.

**Status**: Cierre - tareas completadas y decisiones T001-T003 resueltas.

**Tests**: No se incluyen tests de aplicacion en esta fase. Se incluyen checks
documentales seguros y tareas de revision.

## Phase 1: Decisiones y preparacion

**Purpose**: Confirmar las decisiones que afectan la estructura documental antes
de crear fuentes canonicas.

- [x] T001 Adoptar `docs/ESTADO.md` como fuente unica del estado general,
      zonas, metricas y ciclo de vida de las specs.
- [x] T002 Adoptar checklist para cambios pequenos y localizados; usar
      `spec/plan/tasks` para iniciativas medianas o grandes o con impacto en
      contratos, datos, permisos, storage, documentos o integraciones.
- [x] T003 Adoptar la politica de `docs/archive/`: historia sin autoridad
      operativa, consulta explicita, revision de referencias y aprobacion antes
      de mover documentos.
- [x] T004 Inventariar referencias existentes a `legacy-baseline.md`,
      `legacy-audit.md`, `governance-proposal.md` y `resources/views/docs/` antes
      de crear enlaces o duplicados.

**Checkpoint**: Las decisiones pendientes que bloqueen la estructura quedan
registradas como `REQUIRES TEAM INFO` o `REQUIRES DECISION`.

## Phase 2: Fundacion documental

**Purpose**: Crear la documentacion tecnica viva sin modificar el runtime.

- [x] T005 [US1] Crear `docs/ESTADO.md` como indice de estado operativo,
      backlog, deuda, validaciones y decisiones abiertas, sin duplicar el
      inventario completo de `legacy-baseline.md`.
- [x] T006 [US1] Crear `docs/modelo-de-datos.md` con tablas, relaciones,
      migraciones y estado de evidencia; marcar el esquema productivo como
      pendiente cuando no pueda confirmarse.
- [x] T007 [US1] Crear `docs/rutas-y-accesos.md` con rutas, middleware,
      permisos, roles y ownership observados; separar evidencia de inferencias.
- [x] T008 [US1] Crear `docs/storage-y-archivos.md` con discos, paths,
      consumidores y riesgos conocidos, sin listar contenido sensible.
- [x] T009 [US1] Crear `docs/integraciones-y-operaciones.md` para Telegram,
      correo, mapas, Excel, PDF, comandos, scheduler, workers y binarios cuando
      exista evidencia suficiente.
- [x] T010 [US1] Crear `docs/gotchas.md` con problemas reproducibles de
      XAMPP/Windows, configuracion, cache, storage, documentos y tests.
- [x] T011 [US1] Crear `docs/archive/README.md` definiendo que el archivo es
      historico y no fuente del estado vigente; no mover documentos en esta
      tarea.

**Checkpoint**: Cada documento nuevo tiene una responsabilidad unica, evidencia
o clasificacion, y no altera `resources/views/docs/`.

## Phase 3: Integracion del flujo SpecKit

**Purpose**: Hacer descubrible el proceso para futuras iniciativas.

- [x] T012 [US2] Actualizar `AGENTS.md` en su seccion de documentos de referencia
      para enlazar `docs/` y `specs/`, sin duplicar el baseline.
- [x] T013 [US2] Revisar `.opencode/commands/config_spec.md` contra la
      estructura final y actualizar solo paths o reglas que hayan cambiado.
- [x] T014 [US2] Documentar en `docs/ESTADO.md` que la iniciativa
      `gestion-documental` es propuesta, implementada o pendiente, con fecha y
      evidencia.
- [x] T015 [US2] Definir una checklist reusable de cierre documental para
      iniciativas, incluyendo diff, status, contratos afectados y validaciones
      no ejecutadas.
- [x] T027 [US2] Adaptar `.opencode/agents/implement.md` para ejecutar por
      `tasks.md`, actualizar `docs/ESTADO.md` y bloquear operaciones destructivas.

**Checkpoint**: Una nueva feature puede seguir el flujo del comando sin
depender de documentos de `lms-test`.

## Phase 4: Auditoria read-only

**Purpose**: Añadir control de consistencia cuando existan fuentes canonicas.

- [x] T016 [US3] Crear `.opencode/agents/doc-audit.md` en modo read-only,
      limitado a comparar documentacion con codigo y configuracion.
- [x] T017 [US3] Definir las fuentes que el auditor debe cruzar:
      `AGENTS.md`, Constitution, baseline, audit, `docs/`, specs, rutas,
      migraciones, tests y comandos relevantes.
- [x] T018 [US3] Definir formato de salida del auditor: archivo, simbolo o
      linea, afirmacion, evidencia, clasificacion y correccion sugerida.
- [x] T019 [US3] Validar que el auditor no modifique codigo, documentos,
      storage, base de datos ni configuracion y que reporte incertidumbres.

**Checkpoint**: La auditoria puede ejecutarse sin efectos secundarios.

## Phase 5: Validacion y cierre

**Purpose**: Verificar la consistencia y dejar el estado documentado.

- [x] T020 Revisar los documentos creados contra
      `specs/gestion-documental/spec.md` y `plan.md`.
- [x] T021 Buscar duplicaciones de estado, metricas o hashes y registrar una
      fuente de precedencia cuando sea necesario.
- [x] T022 Confirmar que `resources/views/docs/` no fue movido, renombrado ni
      modificado por la iniciativa.
- [x] T023 Revisar que no existan secretos, backups, logs sensibles o datos
      personales innecesarios en los documentos.
- [x] T024 Ejecutar `git diff` y `git status`; registrar modificaciones no
      relacionadas sin revertirlas.
- [x] T025 Documentar validaciones ejecutadas y no ejecutadas. No ejecutar
      migraciones, restores, backups, limpiezas ni tests contra una base no
      aislada.
- [x] T026 Registrar en `docs/ESTADO.md` el ciclo de vida de las specs y la
      iniciativa `gestion-documental` en estado `Implementacion`, manteniendo
      pendientes las decisiones T001-T003.

## Dependencies & Execution Order

### Phase Dependencies

- **Phase 1** bloquea la creacion de fuentes canonicas si el equipo no decide la
  fuente unica de estado o la politica de archivado.
- **Phase 2** depende de las decisiones aplicables de Phase 1.
- **Phase 3** depende de que las responsabilidades de `docs/` esten definidas.
- **Phase 4** depende de que existan fuentes canonicas y un formato de salida.
- **Phase 5** depende de todas las fases que hayan sido aprobadas para esta
  iniciativa.

### User Story Dependencies

- **US1**: Phase 1 y Phase 2.
- **US2**: Phase 1 y Phase 3.
- **US3**: Phase 2 y Phase 4.

## Definition of Done

- [x] Las decisiones bloqueantes estan aprobadas o marcadas explicitamente como
      pendientes no bloqueantes.
- [x] Cada documento tecnico tiene una responsabilidad y una fuente de
      precedencia clara.
- [x] `resources/views/docs/` continua separado y sin cambios incidentales.
- [x] Las iniciativas futuras tienen un flujo SpecKit reproducible.
- [x] La auditoria documental es read-only.
- [x] No se ejecutaron operaciones destructivas ni tests no seguros.
- [x] `git diff` y `git status` fueron revisados.
- [x] El reporte final distingue cambios, no cambios, validaciones, riesgos y
      decisiones pendientes.
