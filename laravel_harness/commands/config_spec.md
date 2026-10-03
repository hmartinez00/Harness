---
description: Crea la especificacion completa de una feature (spec, plan y tasks) sin implementar cambios de aplicacion.
agent: build
---

# Flujo de especificacion de Sugycom

Ejecuta este flujo para documentar una iniciativa nueva usando los templates
SpecKit del proyecto. Este comando crea o actualiza exclusivamente documentos de
la iniciativa y, solo si se solicita de forma explicita y el entorno es seguro,
tests de caracterizacion. No implementes cambios de aplicacion.

## Paso 0: Recopilar el alcance

Si faltan datos, pregunta antes de crear archivos:

1. Nombre de la feature en `kebab-case`.
2. Descripcion breve y resultado esperado.
3. Usuarios, roles y permisos implicados.
4. Contexto adicional: rutas, modelos, vistas, documentos, integraciones o
   decisiones previas relevantes.

Si el usuario entrega un plan, prompt, issue o borrador, usalo como entrada,
pero no lo conviertas automaticamente en un hecho confirmado.

Clasifica durante el analisis cualquier incertidumbre como:

- `CONFIRMED`
- `INFERRED`
- `REQUIRES TEAM INFO`
- `REQUIRES DECISION`
- `CONFLICT`
- `UNKNOWN`

## Paso 1: Inspeccionar el flujo completo

Antes de redactar, revisa solo lo relevante para la feature:

1. `AGENTS.md` y `.specify/memory/constitution.md`.
2. `legacy-baseline.md`, `legacy-audit.md` y, si aplica,
   `governance-proposal.md`.
3. Rutas, nombres de rutas, middleware, redirects y endpoints AJAX.
4. Controllers, Form Requests, policies, permisos Spatie, roles y ownership.
5. Models, relaciones, migraciones, seeders y datos persistidos.
6. Vistas Blade, JavaScript, imports/exports y generacion de PDF u otros
   documentos.
7. Storage, discos, paths, descargas y archivos existentes.
8. Commands, scheduler, workers e integraciones externas cuando correspondan.
9. Tests existentes y configuracion efectiva del entorno.
10. `docs/` y `specs/` existentes, si ya fueron creados.

No asumas que el entorno local representa produccion. No asumas que la
existencia de una ruta implica consumidores externos.

No trates `resources/views/docs/` como documentacion tecnica del repositorio:
es una superficie funcional de la aplicacion y debe conservarse como tal salvo
que la feature lo afecte explicitamente.

## Paso 2: Crear `spec.md`

Crea `specs/{feature-name}/spec.md` usando el template local de
`.specify/templates/spec-template.md`, adaptado al dominio de Sugycom.

Incluye como minimo:

- contexto y estado actual verificable;
- user stories priorizadas como `P1`, `P2` o `P3`;
- escenarios de aceptacion Given/When/Then;
- requisitos funcionales `FR-xxx`;
- entidades y contratos afectados;
- criterios de exito medibles `SC-xxx`;
- casos limite y errores;
- supuestos explicitamente marcados;
- permisos, ownership y actores autorizados;
- rutas, parametros, JSON, imports/exports y documentos afectados;
- tablas, migraciones, storage e integraciones afectadas;
- matriz de hechos, inferencias y decisiones pendientes;
- tratamiento de compatibilidad: `PRESERVE`, `CHARACTERIZE`, `CHANGE` o
  `UNKNOWN`.

No inventes requisitos de produccion, consumidores externos, permisos reales ni
semantica de ownership.

## Paso 3: Crear `plan.md`

Crea `specs/{feature-name}/plan.md` usando
`.specify/templates/plan-template.md` y documenta los paths reales del
proyecto.

El contexto tecnico debe reflejar el repositorio actual, no LMS:

- PHP y Laravel: consultar `composer.json` y configuracion efectiva;
- frontend: Blade, Bootstrap/Tailwind/Alpine u otras dependencias realmente
  usadas por la feature;
- almacenamiento: MySQL local y cualquier storage o disco afectado;
- tests: PHPUnit/Pest y el entorno seguro disponible;
- plataforma: Windows/XAMPP local y diferencias conocidas con produccion;
- integraciones, binarios, scheduler y workers, si aplican.

Incluye una comprobacion de la Constitution con estas puertas:

| Area | Estado | Evidencia o pendiente |
|---|---|---|
| Compatibilidad de rutas y redirects | Pasa/Falta | ... |
| Autenticacion, permisos y ownership | Pasa/Falta | ... |
| Datos, schema y migraciones | Pasa/Falta | ... |
| Storage, archivos y documentos | Pasa/Falta | ... |
| Imports/exports e integraciones | Pasa/Falta/N/A | ... |
| Comandos, scheduler y operabilidad | Pasa/Falta/N/A | ... |
| Seguridad y proteccion de datos | Pasa/Falta | ... |
| Validacion segura y rollback | Pasa/Falta | ... |

Si una puerta depende de `REQUIRES TEAM INFO` o `REQUIRES DECISION`, no la
resuelvas silenciosamente: dejala como pendiente y describe la pregunta
concreta.

## Paso 4: Crear `tasks.md`

Crea `specs/{feature-name}/tasks.md` a partir del template local, sustituyendo
todos los ejemplos por tareas reales y con paths exactos.

Organiza las tareas por fase e historia de usuario. Cada tarea debe indicar:

- identificador unico `T001`, `T002`, etc.;
- historia relacionada cuando aplique;
- dependencias;
- archivos concretos;
- validacion asociada;
- impacto documental.

Incluye, cuando corresponda:

1. inspeccion y caracterizacion;
2. tests o checks seguros;
3. autorizacion;
4. modelos y migraciones compatibles;
5. controllers, requests y rutas;
6. vistas, JavaScript y documentos;
7. storage e integraciones;
8. actualizacion de `docs/` y `AGENTS.md` solo si el cambio realmente lo
   requiere;
9. revision final de diff, status y contratos afectados.

No copies prefijos historicos de otro proyecto ni supongas que `Hxxx` es una
convencion de Sugycom. Usa `Txxx` salvo que el equipo defina otra regla.

## Paso 4.1: Registrar el ciclo de vida

Actualiza `docs/ESTADO.md` con la iniciativa y su estado canonico:

- **Diseno**: spec, plan y tasks creados y revisados; aun no se implementa.
- **Implementacion**: hay tareas o cambios en curso.
- **Cierre**: la implementacion termino, fue revisada y tiene referencia de
  entrega.

`docs/ESTADO.md` es la fuente unica del estado general y de las specs. El campo
`Status` de `spec.md`, `plan.md` o `tasks.md` debe mantenerse alineado, pero no
puede sustituir el registro central. Actualiza tambien las zonas y metricas de
`docs/ESTADO.md`; no dupliques numeros o hashes en otros documentos.

## Paso 5: Tests de caracterizacion o TDD

Por defecto, esta fase es documental y no crea tests ejecutables.

Solo crea tests si el usuario lo solicita expresamente o la especificacion los
declara necesarios. En ese caso:

- inspecciona primero `tests/`, `tests/Pest.php`, `phpunit.xml` y el entorno;
- no uses SQLite como sustituto automatico de MySQL ni como evidencia de
  produccion;
- no ejecutes tests contra una base con datos reales o no aislada;
- no generes intencionalmente tests rotos para simular TDD salvo solicitud
  explicita y un entorno aislado;
- documenta si el test fue escrito, ejecutado, omitido o bloqueado;
- no presentes como validado un test que no pudo ejecutarse de forma segura.

## Paso 6: Revision final y reporte

Antes de terminar:

1. Revisa los tres documentos completos.
2. Comprueba que no haya secretos, datos personales innecesarios ni contenido
   de backups o logs.
3. Verifica que cada afirmacion importante tenga evidencia o clasificacion.
4. Comprueba que las rutas a archivos existan o queden marcadas como pendientes.
5. Revisa `git diff` y `git status`.
6. No ejecutes migraciones, restores, backups, limpiezas ni operaciones sobre
   datos reales.
7. No hagas commit.

Reporta:

- archivos creados o modificados;
- decisiones y supuestos adoptados;
- elementos `REQUIRES TEAM INFO`, `REQUIRES DECISION`, `CONFLICT` y `UNKNOWN`;
- validaciones ejecutadas y no ejecutadas;
- cualquier hallazgo fuera del alcance.
