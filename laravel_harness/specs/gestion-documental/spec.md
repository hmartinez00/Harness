# Feature Specification: Gestion documental del repositorio

**Feature Branch**: `gestion-documental`
**Created**: 2026-09-30
**Status**: Cierre
**Input**: Plan de adaptacion de la metodologia documental observada en
`C:\laragon\www\lms-test`, adaptado a las reglas de Sugycom.

## Contexto

### 1. Estado actual

El repositorio tiene documentacion y reglas relevantes repartidas en:

- `AGENTS.md`, con instrucciones operativas para la modernizacion incremental.
- `.specify/memory/constitution.md`, con principios de gobierno en estado draft.
- `legacy-baseline.md`, con el baseline verificable y sus incertidumbres.
- `legacy-audit.md`, con evidencia y observaciones tecnicas.
- `governance-proposal.md`, con una propuesta separada de los hechos confirmados.
- `.specify/templates/`, con templates base de SpecKit.
- `resources/views/docs/operations/` y `resources/views/docs/dlinx.site/`, que
  contienen documentacion funcional servida por la aplicacion.

No se ha confirmado una estructura tecnica activa de `docs/` ni un registro
`specs/` previo. Esta iniciativa propone organizar la documentacion tecnica sin
mover ni reinterpretar el contenido funcional servido por `/docs/{slug?}`.

### 2. Alineacion con documentos de referencia

La iniciativa debe preservar las siguientes reglas ya observadas:

- compatibilidad inicial de rutas, datos, storage, documentos e integraciones;
- distincion entre hechos, inferencias, informacion del equipo y decisiones;
- no asumir que Windows/XAMPP representa produccion;
- no ejecutar operaciones destructivas sobre datos reales;
- mantener separada la Constitution de las instrucciones operativas y del
  baseline historico/verificable.

### 3. Decisiones adoptadas para esta especificacion

- La documentacion tecnica nueva vivira en `docs/`.
- El estado operativo actual tendra una fuente unica en `docs/ESTADO.md`.
- Las iniciativas medianas o grandes usaran `specs/<feature-name>/` con
  `spec.md`, `plan.md` y `tasks.md`.
- `resources/views/docs/` continuara tratandose como superficie funcional de la
  aplicacion, no como documentacion interna del repositorio.
- Las tareas usaran identificadores `T001`, `T002`, etc., sin copiar prefijos
  historicos de `lms-test`.
- La primera fase no modifica el comportamiento de la aplicacion ni migra
  automaticamente todo el contenido de los documentos legacy.
- El estado general de esta iniciativa se registra en
  `docs/ESTADO.md`, que prevalece sobre este campo `Status` si hubiera deriva.
- El estado de esta iniciativa es `Cierre`: las tareas documentales y de
  configuracion terminaron, las decisiones T001-T003 fueron adoptadas y las
  incertidumbres de produccion restantes no bloquean este alcance.

## User Scenarios & Testing

### User Story 1 - Encontrar el estado documental vigente (Priority: P1)

Como mantenedor del repositorio, quiero saber donde consultar el estado actual,
las reglas de trabajo, los contratos de compatibilidad y las decisiones abiertas
sin tener que reconstruirlos desde varios documentos historicos.

**Why this priority**: Sin una fuente de estado clara, la documentacion puede
contradecir el codigo o convertir hallazgos antiguos en reglas actuales.

**Independent Test**: Una persona nueva puede localizar en menos de diez minutos
el estado operativo, el baseline, las incertidumbres y la fuente canonica de
rutas, datos y permisos siguiendo `AGENTS.md`.

**Acceptance Scenarios**:

1. **Given** el repositorio contiene documentacion tecnica y de gobierno,
   **When** el mantenedor busca el estado actual,
   **Then** `AGENTS.md` enlaza a una fuente unica y no ambigua.
2. **Given** una metrica o estado de feature cambia,
   **When** se actualiza la documentacion,
   **Then** el cambio se realiza en la fuente canonica y no se duplica sin
   justificacion en documentos secundarios.

### User Story 2 - Documentar una iniciativa antes de implementarla (Priority: P1)

Como responsable de una iniciativa, quiero crear una especificacion, un plan y
una lista de tareas con evidencia del sistema actual antes de modificar codigo.

**Why this priority**: Los cambios afectan rutas, permisos, datos, storage y
documentos que deben caracterizarse antes de romper compatibilidad.

**Independent Test**: Una feature nueva puede generar `spec.md`, `plan.md` y
`tasks.md` con paths concretos, riesgos, dependencias y validaciones sin crear
cambios de aplicacion.

**Acceptance Scenarios**:

1. **Given** una feature tiene nombre y alcance suficiente,
   **When** se ejecuta el flujo `/config_spec`,
   **Then** se crea `specs/<feature-name>/` con los tres documentos.
2. **Given** una decision depende del equipo o del entorno productivo,
   **When** se redacta el plan,
   **Then** permanece marcada como `REQUIRES TEAM INFO` o `REQUIRES DECISION`
   con una pregunta concreta.

### User Story 3 - Auditar contradicciones documentales (Priority: P2)

Como mantenedor, quiero revisar la documentacion tecnica contra rutas,
migraciones, permisos, tests y configuracion sin modificarla automaticamente.

**Why this priority**: La documentacion solo es util si su divergencia puede
detectarse sin introducir cambios no revisados.

**Independent Test**: Una futura auditoria read-only puede listar documentos
desactualizados, contradicciones, evidencias y correcciones sugeridas sin tocar
codigo, base de datos ni documentos.

**Acceptance Scenarios**:

1. **Given** un documento declara una ruta, permiso o tabla,
   **When** se compara con el repositorio,
   **Then** el resultado identifica evidencia y clasificacion del hallazgo.
2. **Given** una documentacion funcional bajo `resources/views/docs/` es revisada,
   **When** el auditor la encuentra,
   **Then** la trata como superficie runtime y no la mueve al archivo tecnico.

### Edge Cases

- El repositorio no tiene todavia `docs/` o `specs/`.
- Un dato aparece en `legacy-baseline.md` y en un documento operativo con
  valores distintos.
- Una ruta existe, pero no se puede demostrar que tenga consumidores externos.
- La autorizacion real, ownership, scheduler o motor productivo no pueden
  confirmarse localmente.
- Una feature afecta una ruta de `/docs/{slug?}` o un Markdown funcional.
- Un documento historico contiene informacion sensible o referencias a datos
  operativos.
- Un test requiere una base distinta a la configuracion segura disponible.

## Requirements

### Functional Requirements

- **FR-001**: El repositorio MUST tener un indice documental que distinga
  gobierno, reglas operativas, baseline, auditoria, estado, specs y contenido
  funcional de la aplicacion.
- **FR-002**: `docs/ESTADO.md` MUST ser la fuente canonica del estado operativo,
  backlog, deuda y validaciones cuando sea creado.
- **FR-003**: La documentacion tecnica MUST centralizar rutas, accesos, modelo de
  datos, storage, integraciones y gotchas en documentos con responsabilidad
  definida.
- **FR-004**: Cada feature documentada MUST poder representarse en
  `specs/<feature-name>/` con `spec.md`, `plan.md` y `tasks.md`.
- **FR-005**: Cada spec MUST clasificar hechos, inferencias, incertidumbres,
  decisiones y conflictos, y declarar el tratamiento de compatibilidad.
- **FR-006**: Cada plan MUST incluir puertas de compatibilidad, autorizacion,
  datos, storage, integraciones, operabilidad, seguridad y validacion.
- **FR-007**: Cada lista de tareas MUST usar paths concretos, dependencias,
  validaciones y tareas documentales cuando el impacto lo requiera.
- **FR-008**: La documentacion MUST distinguir contenido tecnico del repositorio
  de los documentos funcionales servidos desde `resources/views/docs/`.
- **FR-009**: El flujo documental MUST evitar migraciones, restauraciones,
  backups, limpiezas y tests contra bases no aisladas.
- **FR-010**: Los documentos nuevos MUST evitar secretos, datos personales
  innecesarios, backups, logs sensibles y valores reales de configuracion.
- **FR-011**: Los documentos historicos que dejen de representar el estado actual
  SHOULD moverse a `docs/archive/` solo despues de revisar sus referencias.
- **FR-012**: La futura auditoria documental SHOULD operar en modo read-only y
  reportar evidencia, contradicciones y correcciones sugeridas.

### Key Entities

- **Estado documental**: estado operativo, backlog, deuda, validaciones y
  decisiones abiertas del repositorio.
- **Contrato tecnico**: ruta, permiso, tabla, formato, path, integracion o
  comportamiento que requiere preservacion o caracterizacion.
- **Iniciativa**: feature documentada mediante spec, plan y tasks.
- **Evidencia**: archivo, simbolo, ruta, test o configuracion que respalda una
  afirmacion.
- **Decision pendiente**: asunto clasificado como `REQUIRES TEAM INFO` o
  `REQUIRES DECISION` que no debe resolverse por inferencia.

## Success Criteria

### Measurable Outcomes

- **SC-001**: Un mantenedor puede localizar la fuente canonica de estado, rutas,
  accesos, datos y gotchas desde `AGENTS.md` sin consultar el otro proyecto.
- **SC-002**: Una iniciativa nueva puede producir sus tres documentos SpecKit
  con paths, riesgos y validaciones definidos antes de implementar.
- **SC-003**: Ningun documento nuevo duplica una metrica o estado sin declarar la
  fuente de precedencia.
- **SC-004**: Una auditoria read-only puede identificar cada afirmacion importante
  sin modificar codigo, configuracion, storage o base de datos.
- **SC-005**: La reorganizacion documental no cambia rutas, permisos, storage,
  documentos funcionales ni comportamiento de `/docs/{slug?}`.

## Assumptions

- La Constitution seguira siendo draft hasta que el equipo registre una
  ratificacion explicita.
- `legacy-baseline.md` y `legacy-audit.md` continuaran siendo las fuentes de
  baseline y evidencia, respectivamente.
- La nueva carpeta `docs/` sera tecnica y no sera consumida automaticamente por
  `DocController`.
- El flujo inicial no requiere una migracion de todos los documentos existentes.
- Los tests de esta iniciativa son checks documentales o de estructura; no se
  requieren tests de aplicacion en esta fase.

## Decisions and Non-blocking Questions

| Question or decision | Classification | Required action |
|---|---|---|
| `docs/ESTADO.md` como fuente unica del estado general | `RESOLVED` | Adoptado como decision T001. |
| Umbral entre checklist y ciclo SpecKit | `RESOLVED` | Adoptado como decision T002. |
| Politica de archivado | `RESOLVED` | Adoptada como decision T003. |
| Fecha de ratificacion de la Constitution | `REQUIRES TEAM INFO` | Es una decision de gobierno del proyecto, no un bloqueo de esta iniciativa. |
| Consumidores externos de rutas o documentos | `REQUIRES TEAM INFO` | Confirmar fuera del repositorio antes de romper contratos. |
| Motor, esquema y scheduler productivos | `REQUIRES TEAM INFO` | Validar con el equipo; no inferir desde XAMPP. |
