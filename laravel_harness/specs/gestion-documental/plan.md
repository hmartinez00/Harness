# Implementation Plan: Gestion documental del repositorio

**Branch**: `gestion-documental` | **Date**: 2026-09-30 | **Spec**:
[`./spec.md`](./spec.md)

**Input**: Feature specification from
`/specs/gestion-documental/spec.md`.

**Lifecycle**: Cierre. Las decisiones T001-T003 estan adoptadas; las preguntas
de produccion restantes pertenecen al backlog general y no bloquean esta
iniciativa documental.

## Summary

1. Definir y documentar las fuentes canonicas de informacion sin mover la
   documentacion funcional de `resources/views/docs/`.
2. Crear una estructura tecnica minima bajo `docs/` para estado, contratos,
   operaciones, gotchas e historial.
3. Adoptar `specs/<feature-name>/` como paquete de documentacion para features
   medianas y grandes.
4. Actualizar el indice operativo en `AGENTS.md` solo con enlaces y reglas que
   no dupliquen el baseline.
5. Incorporar una auditoria documental read-only como iniciativa posterior,
   despues de ratificar las fuentes canonicas.

La implementacion propuesta es documental. No requiere cambios de rutas,
controllers, modelos, migraciones, storage runtime ni comportamiento de
`/docs/{slug?}`.

## Technical Context

**Language/Version**: PHP `^8.2`, Laravel `^12.0`; la iniciativa documental no
introduce codigo PHP de aplicacion.

**Primary Dependencies**: Blade y Markdown ya usados por la aplicacion;
SpecKit bajo `.specify/templates/`; OpenCode para el comando
`.opencode/commands/config_spec.md`.

**Storage**: MySQL local declarado por las reglas del proyecto; el motor y
esquema productivos son `REQUIRES TEAM INFO`. Esta iniciativa no toca la base de
datos ni crea migraciones.

**Testing**: No se requieren tests de aplicacion. Se realizaran checks seguros
de estructura, enlaces, contenido sensible y consistencia documental. La
configuracion Pest/PHPUnit existe en `composer.json` y `phpunit.xml`, pero no se
usara como sustituto de validacion documental.

**Target Platform**: Repositorio Laravel ejecutado localmente en Windows/XAMPP;
la documentacion no debe inferir comportamiento de produccion desde ese
entorno.

**Project Type**: Aplicacion web Laravel legacy con vistas Blade, AdminLTE,
integraciones y documentos operativos existentes.

**Performance Goals**: La estructura documental no debe alterar el tiempo de
respuesta ni el indice funcional de `/docs/{slug?}`. No se define una metrica de
runtime para esta fase.

**Constraints**:

- No mover, renombrar ni eliminar `resources/views/docs/` en esta iniciativa.
- No modificar rutas, middleware, permisos, modelos, migraciones, storage,
  scheduler o integraciones de la aplicacion.
- No copiar secretos, datos personales, backups, logs o valores reales de
  configuracion.
- No convertir inferencias de `legacy-audit.md` en hechos normativos.
- No resolver silenciosamente `REQUIRES TEAM INFO` o `REQUIRES DECISION`.
- No ejecutar migraciones, restores, backups, limpiezas ni tests contra una base
  no aislada.

**Scale/Scope**: Documentacion tecnica inicial y paquete SpecKit de una
iniciativa; sin cambios de comportamiento y sin migracion masiva de contenido.

## Constitution Check

| Area | Estado | Evidencia o pendiente |
|---|---|---|
| Compatibilidad de rutas y redirects | Pasa | La iniciativa no modifica rutas. `routes/web.php` y el contrato `/docs/{slug?}` se conservan. |
| Autenticacion, permisos y ownership | Pasa con caracterizacion | Se documentaran como contratos; los permisos reales y ownership productivo siguen siendo `REQUIRES TEAM INFO`. |
| Datos, schema y migraciones | Pasa | No se modifican tablas, datos ni migraciones. El motor productivo queda pendiente. |
| Storage, archivos y documentos | Pasa con restriccion | No se mueven archivos. `resources/views/docs/` queda fuera de la documentacion tecnica nueva. |
| Imports/exports e integraciones | N/A en fase inicial | Se documentaran como contratos solo si una iniciativa posterior los afecta. |
| Comandos, scheduler y operabilidad | Pasa con pendiente | No se cambia operacion; scheduler y binarios productivos requieren confirmacion posterior. |
| Seguridad y proteccion de datos | Pasa | Los documentos no contendran secretos ni contenido sensible. |
| Validacion segura y rollback | Pasa | La fase es documental y reversible mediante versionado; no toca datos ni runtime. |

**Estado**: PASA para la fase documental. Quedan pendientes las decisiones y
validaciones expresamente clasificadas en `spec.md`.

## Project Structure

### Documentation for this initiative

```text
specs/gestion-documental/
├── spec.md       # alcance, escenarios, requisitos y decisiones pendientes
├── plan.md       # contexto tecnico, puertas y fases
└── tasks.md      # tareas ejecutables y criterios de validacion
```

### Proposed technical documentation structure

```text
docs/
├── ESTADO.md
├── modelo-de-datos.md
├── rutas-y-accesos.md
├── storage-y-archivos.md
├── integraciones-y-operaciones.md
├── gotchas.md
├── plan-implementacion.md
└── archive/
    └── README.md
```

`docs/` es una propuesta de fase posterior. No se crea automaticamente como
parte de esta especificacion sin ejecutar las tareas correspondientes.

### Application documentation kept separate

```text
resources/views/docs/
├── operations/
└── dlinx.site/
```

Estos archivos forman parte del contenido funcional renderizado por
`App\Http\Controllers\DocController` y no deben archivarse ni trasladarse sin
una feature especifica que caracterice sus consumidores.

## Implementation Phases

### Phase 0: Decision and inventory

- Confirmar la fuente unica de estado operativo.
- Confirmar el alcance de SpecKit para cambios pequenos.
- Confirmar la politica de archivado.
- Inventariar referencias a documentos legacy antes de crear duplicados.

### Phase 1: Minimal documentation foundation

- Crear los documentos canonicos de `docs/` con alcance acotado.
- Mantener cada documento pequeno y con referencias a evidencia concreta.
- Registrar en `docs/ESTADO.md` solo hechos y estado operativo, no toda la
  auditoria historica.

### Phase 2: Process integration

- Actualizar `AGENTS.md` con un indice de navegacion hacia `docs/` y `specs/`.
- Usar `.opencode/commands/config_spec.md` para nuevas iniciativas.
- Definir una checklist de finalizacion documental.

### Phase 3: Read-only audit

- Crear `.opencode/agents/doc-audit.md` cuando las fuentes canonicas existan.
- Auditar contradicciones entre documentos y codigo sin editar automaticamente.
- Registrar hallazgos como backlog, riesgo, deuda o pregunta pendiente.

## Technical Decisions

### Decision 1: Mantener separada la documentacion funcional

`resources/views/docs/` es consumida por `DocController` y forma parte de la
aplicacion. Mezclarla con `docs/` podria cambiar URLs, indice, lenguaje o
contenido de usuario. Se mantiene en su ubicacion hasta caracterizar cualquier
cambio futuro.

### Decision 2: No migrar todo el baseline a `docs/`

`legacy-baseline.md` y `legacy-audit.md` tienen funciones distintas a un estado
operativo. Se enlazaran desde `AGENTS.md` y desde `docs/ESTADO.md`, evitando
duplicar inventarios y clasificaciones.

### Decision 3: Usar `Txxx` para tareas nuevas

No existe evidencia de una convencion local previa de prefijos. `Txxx` coincide
con el template actual y evita importar convenciones historicas de LMS.

### Decision 4: Tests fuera del alcance inicial

La iniciativa no cambia codigo ejecutable. Los checks seran de estructura,
referencias y consistencia; cualquier test de aplicacion requiere una feature
concreta y un entorno aislado.

## Validation Plan

- Verificar que los nuevos documentos enlacen a paths existentes o marquen la
  referencia como pendiente.
- Buscar duplicacion de metricas y estados entre `docs/ESTADO.md`, baseline,
  auditoria y specs.
- Confirmar que `resources/views/docs/` no haya sido movido o modificado por la
  fase documental.
- Revisar ausencia de secretos, backups, logs y datos personales innecesarios.
- Ejecutar `git diff` y `git status`.
- No ejecutar PHPUnit/Pest ni comandos Artisan porque no son necesarios para
  validar documentos y el entorno de base de datos no se ha confirmado como
  aislado.

## Open Decisions

- Ratificacion de `.specify/memory/constitution.md` y fecha efectiva. Esto es
  gobierno general del proyecto, no un bloqueo de `gestion-documental`.
- Confirmacion de consumidores externos, motor productivo, permisos reales,
  ownership, scheduler y politica de storage.

## Complexity Tracking

No se introduce complejidad arquitectonica ni una nueva capa de aplicacion. La
estructura propuesta agrega documentos y un futuro auditor read-only, ambos
justificados por trazabilidad y control de regresiones documentales.
