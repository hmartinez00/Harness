# Registro historico: arnes de agentes y gestion documental

**Fecha de registro**: 2026-09-30
**Repositorio**: `sugycom_v3_0_1`
**Estado**: historico; describe la configuracion incorporada al proyecto y no
es la fuente del estado actual. El estado vigente se consulta en
`docs/ESTADO.md`.

## 1. Proposito

Este documento conserva los detalles de los elementos incorporados para formar
el arnes de trabajo asistido del proyecto:

1. skills para orientar el trabajo del agente;
2. MCP Context7 para documentacion actualizada de librerias;
3. flujo SpecKit para especificar iniciativas sin implementar codigo;
4. gestion documental con estado general, specs por iniciativa y auditoria
   read-only.

El arnes no forma parte del runtime Laravel. Sus archivos viven en carpetas de
proceso como `.agents/`, `.claude/`, `.opencode/`, `docs/` y `specs/`.

## 2. Skills

### 2.1 `frontend-design`

**Origen**: `https://github.com/anthropics/skills`
**Skill**: `frontend-design`
**Instalacion usada**:

```bash
npx skills add https://github.com/anthropics/skills --skill frontend-design -y
```

La instruccion original del proyecto estaba en `prompt.md`. El CLI registro la
skill en el alcance del proyecto y creo las integraciones para agentes
compatibles.

**Archivos persistidos**:

- `.agents/skills/frontend-design/SKILL.md`
- `.agents/skills/frontend-design/LICENSE.txt`
- `.claude/skills/frontend-design/SKILL.md`
- `.claude/skills/frontend-design/LICENSE.txt`
- `skills-lock.json`

**Alcance**:

- definir una direccion visual especifica al dominio;
- establecer tokens de color, tipografia, layout y principios;
- evitar disenos genericos o repetitivos;
- revisar responsive, accesibilidad, foco visible y reduced motion;
- criticar el resultado antes de cerrar una implementacion frontend.

La skill no modifica por si misma vistas ni assets. Sus instrucciones se aplican
cuando una tarea de frontend lo requiere.

### 2.2 Skills internas de operacion

Durante la instalacion y configuracion se cargaron skills internas del entorno
del agente para realizar operaciones especializadas:

- `find-skills`: proceso para descubrir e instalar skills mediante `npx skills`.
- `customize-opencode`: reglas para configurar OpenCode, especialmente
  `opencode.json`, MCP, comandos y agentes.

Estas skills son proporcionadas por el entorno del agente y no fueron copiadas
como dependencias del proyecto. No deben documentarse como archivos locales del
repositorio ni confundirse con `frontend-design`, que si esta persistida bajo
`.agents/skills/`.

## 3. MCP Context7

### 3.1 Configuracion persistida

Archivo: `opencode.json`

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "context7": {
      "type": "remote",
      "url": "https://mcp.context7.com/mcp",
      "enabled": true
    }
  }
}
```

### 3.2 Proveedor y capacidades

- Proveedor oficial listado: Upstash/Context7.
- Conexion: MCP remoto sobre HTTPS.
- Endpoint: `https://mcp.context7.com/mcp`.
- Herramientas esperadas: `resolve-library-id` y `query-docs`.
- Funcion: consultar documentacion y ejemplos actuales de librerias, SDKs,
  APIs, frameworks y herramientas.
- API key: no se guardo ninguna en el repositorio.

Context7 recomienda una API key para mayores limites. Si se incorpora una, debe
provenir de una variable de entorno o mecanismo seguro de OpenCode; nunca debe
escribirse en `opencode.json`, este documento, logs o commits.

### 3.3 Verificacion registrada

Comando:

```bash
opencode mcp list
```

Resultado observado durante la configuracion:

```text
context7 connected
https://mcp.context7.com/mcp
```

Una consulta GET directa al endpoint respondio `405`, comportamiento compatible
con un endpoint MCP que espera solicitudes POST del protocolo. La comprobacion
operativa valida el servidor con `opencode mcp list`.

## 4. Flujo SpecKit y comando local

Se incorporo un comando de OpenCode:

```text
.opencode/commands/config_spec.md
```

Uso:

```text
/config_spec
```

El comando crea o mantiene la documentacion de una iniciativa en:

```text
specs/<feature-name>/
├── spec.md
├── plan.md
└── tasks.md
```

El flujo local exige inspeccionar, segun el alcance, rutas, middleware,
controladores, requests, modelos, migraciones, permisos, storage, imports,
exports, documentos, comandos, scheduler, integraciones y tests.

El flujo no implementa codigo de aplicacion. Los tests solo se documentan o
crean cuando se solicitan explicitamente y el entorno es seguro. No se deben
crear tests deliberadamente rotos ni ejecutarlos contra una base no aislada.

### 4.1 Agente implementador

Se adapto el agente implementador de `C:\laragon\www\lms-test` para Sugycom:

```text
.opencode/agents/implement.md
```

Su fuente unica de alcance es `specs/<feature-name>/tasks.md`. Antes de editar
debe comprobar la iniciativa en `docs/ESTADO.md`, leer spec/plan/tasks,
`AGENTS.md`, la Constitution y los documentos canonicos afectados.

El agente implementador de Sugycom:

- trabaja por fases y no salta tareas ni amplia alcance;
- actualiza `docs/ESTADO.md` al cambiar de fase, validacion o bloqueo;
- distingue codigo implementado, migracion validada, migracion aplicada y
  despliegue operativo;
- respeta compatibilidad de rutas, permisos, datos, storage, formatos,
  documentos, integraciones, comandos y scheduler;
- ejecuta tests solo cuando la tarea lo exige y el entorno es seguro;
- no asume que SQLite equivale a MySQL o produccion;
- no hace commit sin autorizacion explicita.

Sus permisos bloquean migraciones, seeders, backups, restores, limpieza,
`git pull`, `git reset`, `git checkout`, `git clean` y operaciones destructivas.
Los comandos ambiguos quedan en `ask` para el usuario.

## 5. Gestion documental

### 5.1 Fuentes de verdad

- `AGENTS.md`: reglas operativas para agentes.
- `.specify/memory/constitution.md`: principios de gobierno; su ratificacion
  queda pendiente hasta que el equipo la apruebe.
- `legacy-baseline.md`: estado verificable e incertidumbres del sistema legacy.
- `legacy-audit.md`: evidencia y observaciones tecnicas.
- `docs/ESTADO.md`: estado general del proyecto, iniciativas, validaciones y
  zonas de trabajo.
- `docs/`: documentacion tecnica viva por responsabilidad.
- `specs/`: documentacion de iniciativas.
- `.opencode/agents/implement.md`: implementador por tareas y fases, con
  controles de compatibilidad y operaciones seguras.
- `docs/archive/`: historia documental, no estado vigente.
- `resources/views/docs/`: documentacion funcional servida por Laravel; no es
  parte de `docs/` tecnico.

### 5.2 Documentos tecnicos creados

```text
docs/
├── ESTADO.md
├── modelo-de-datos.md
├── rutas-y-accesos.md
├── storage-y-archivos.md
├── integraciones-y-operaciones.md
├── gotchas.md
├── plan-implementacion.md
├── checklist-cierre.md
└── archive/
    └── README.md
```

### 5.3 Ciclo de vida de las specs

`docs/ESTADO.md` registra cada iniciativa en uno de tres estados:

1. **Diseno**: alcance, spec, plan y tasks definidos; sin implementacion
   iniciada.
2. **Implementacion**: tareas o cambios en curso; se registran avances,
   archivos afectados y validaciones parciales.
3. **Cierre**: implementacion terminada, revisada y con validaciones,
   documentacion y referencia de entrega registradas.

La tabla de iniciativas de `docs/ESTADO.md` prevalece sobre etiquetas antiguas
en `spec.md`, `plan.md` o `tasks.md`.

### 5.4 Auditoria documental

Se incorporo un agente read-only:

```text
.opencode/agents/doc-audit.md
```

Su funcion es comparar documentacion con rutas, codigo, migraciones, permisos,
storage, configuracion, tests y comandos. Reporta evidencia, contradicciones,
clasificacion y correcciones sugeridas, pero no modifica archivos ni ejecuta
operaciones destructivas.

## 6. Primera iniciativa registrada

La primera iniciativa del arnes es:

```text
specs/gestion-documental/
├── spec.md
├── plan.md
└── tasks.md
```

Estado al momento de este registro: **Cierre**.

Implementado:

- estructura documental tecnica;
- fuente general `docs/ESTADO.md`;
- ciclo de vida de specs;
- checklist de cierre;
- comando `/config_spec` adaptado a Sugycom;
- agente `doc-audit` read-only.
- agente `implement` por tareas y fases con bloqueos de seguridad.

Decisiones adoptadas para el cierre:

- `docs/ESTADO.md` es la fuente unica del estado general y de las specs;
- los cambios pequenos usan checklist y las iniciativas medianas/grandes usan
  SpecKit;
- `docs/archive/` conserva historia, no gobierna el estado actual, y requiere
  revision y aprobacion antes de mover contenido.

Pendiente no bloqueante:

- ratificar la Constitution y registrar su fecha;
- confirmar consumidores externos, motor productivo, permisos, ownership,
  scheduler, binarios y politica de storage.

## 7. Seguridad y limites

- No se incluyeron API keys, tokens, passwords, valores de `.env` ni datos
  personales.
- No se modificaron rutas, controladores, modelos, migraciones, storage ni
  comportamiento runtime como parte del arnes.
- No se debe tratar una skill o MCP como autoridad para resolver decisiones de
  produccion sin confirmacion del equipo.
- Context7 entrega documentacion externa actualizada, no evidencia del
  comportamiento propio de este repositorio.
- La existencia de skills, MCP, specs o documentos no demuestra que un proceso
  este aprobado para produccion.

## 8. Reproduccion y mantenimiento

Para verificar el arnes:

```bash
npx skills list --json
opencode mcp list
```

Para actualizar la skill instalada, revisar primero los cambios del upstream y
usar el CLI de skills en alcance de proyecto. Para cambios en
`opencode.json`, comandos, agentes o skills, reiniciar OpenCode porque la
configuracion se carga al iniciar.

Este registro es historico. Las modificaciones futuras deben actualizar
`docs/ESTADO.md`, el spec correspondiente y este archivo solo cuando cambie la
composicion del arnes o su forma de reproducirse.

## 9. Modelo portable de gobierno documental

El arnes se abstrae como un modelo de gobierno documental aplicable a otros
proyectos legacy. Esta seccion describe criterios transferibles; no convierte
este archivo historico en una Constitution ni sustituye las reglas del proyecto
que lo adopte.

### 9.1 Jerarquia documental portable

Cada proyecto debe definir una fuente propietaria para cada tipo de informacion
mutable. Los demas documentos deben enlazarla, no duplicarla.

| Pregunta | Fuente canonica | Tipo |
|---|---|---|
| Cual es el estado actual del proyecto | `docs/ESTADO.md` | Vivo |
| Cuales son los principios obligatorios | Constitution del proyecto | Normativo |
| Como debe trabajar un agente | `AGENTS.md` | Operativo |
| Que requiere una iniciativa | `specs/<feature>/spec.md` | Por iniciativa |
| Cual es la solucion tecnica | `specs/<feature>/plan.md` | Por iniciativa |
| Que trabajo ejecutable existe | `specs/<feature>/tasks.md` | Por iniciativa |
| Que se verifico | Informe de validacion o auditoria | Evidencia |
| Que dejo de estar vigente | `docs/archive/` y Git | Historico |

Una implementacion concreta puede usar otros nombres, pero debe conservar la
separacion entre estado vivo, reglas, iniciativas, evidencia e historia.

### 9.2 Directrices constitucionales minimas

Una Constitution de proyecto legacy que adopte este arnes deberia exigir que
cada iniciativa relevante registre, segun corresponda:

- comportamiento actual y contratos existentes;
- contratos que deben preservarse;
- comportamiento pendiente de caracterizacion;
- comportamiento que se va a cambiar;
- riesgos de seguridad, datos, compatibilidad y operacion;
- estrategia de migracion, recuperacion o rollback;
- validaciones ejecutadas y no ejecutadas;
- informacion que requiere al equipo;
- decisiones que requieren aprobacion.

Tambien deberia prohibir convertir silenciosamente:

- inferencias en hechos;
- recomendaciones en reglas;
- riesgos en decisiones aprobadas;
- ausencia de evidencia en inexistencia;
- estado local en evidencia de produccion.

La Constitution debe definir la precedencia sobre `AGENTS.md`, su proceso de
ratificacion y su proceso de enmiendas. Si aun es un draft, el estado debe
aparecer explicitamente en `docs/ESTADO.md`.

### 9.3 Ciclo portable de una iniciativa

El estado canonico de una iniciativa debe registrarse en `docs/ESTADO.md` con
tres estados:

1. **Diseno**: se define el problema, se inspecciona el flujo completo y se
   revisan `spec.md`, `plan.md` y `tasks.md`; aun no se implementa.
2. **Implementacion**: se ejecutan las tareas aprobadas y se registran cambios,
   archivos afectados y validaciones parciales.
3. **Cierre**: la implementacion termino, se revisaron contratos, validaciones,
   riesgos y documentacion, y existe referencia de entrega cuando corresponde.

El cierre debe distinguir al menos estas realidades, sin convertirlas en
estados adicionales obligatorios:

- codigo implementado;
- migracion validada en entorno aislado;
- migracion aplicada en un entorno real con autorizacion;
- cambio desplegado y operativamente verificado.

La aprobacion de una spec o de un plan no autoriza automaticamente operaciones
reales, despliegues, commits ni cambios irreversibles.

## 10. Quality gates portables

Antes de cerrar una iniciativa, evaluar las areas afectadas:

- rutas, URLs, parametros y redirects;
- autenticacion, autorizacion, roles y ownership;
- esquema, datos, migraciones y JSON persistido;
- storage, archivos, paths y descargas;
- imports, exports y formatos de documentos;
- JSON/AJAX o APIs;
- integraciones externas;
- comandos, scheduler, workers y binarios;
- seguridad, secretos, TLS, logs y exposicion de datos;
- tests, builds y validaciones reproducibles;
- despliegue, recuperacion y rollback.

La lista debe adaptarse al proyecto. La obligacion portable es evaluar las areas
afectadas y declarar explicitamente las que no aplican.

Cada gate debe poder marcarse como `CONFIRMED`, `INFERRED`, `REQUIRES TEAM
INFO`, `REQUIRES DECISION`, `CONFLICT` o `UNKNOWN`. Para el comportamiento
existente puede anadirse `PRESERVE`, `CHARACTERIZE`, `CHANGE` o `UNKNOWN`.

## 11. Puertas de aprobacion

El flujo portable recomendado es:

```text
Aprobar spec
    -> aprobar plan
    -> aprobar tasks
    -> autorizar implementacion
    -> autorizar cambios reales de datos u operacion
    -> revisar cierre
```

Cada proyecto debe decidir si las puertas son aprobaciones formales, revisiones
de pull request, confirmaciones de equipo o una combinacion. Nunca debe
suponerse que aprobar el plan autoriza automaticamente:

- migraciones en datos reales;
- cambios de permisos;
- movimiento o eliminacion de archivos;
- backups o restores;
- cambios de scheduler;
- commits o despliegues.

## 12. Evidencia, incertidumbre y auditoria

El agente o flujo documental debe investigar el flujo completo relevante antes
de redactar el plan: rutas, middleware, controladores, requests, modelos,
migraciones, vistas, JavaScript, storage, integraciones, comandos, scheduler y
tests. La lista exacta se adapta al stack.

El auditor documental portable debe:

- funcionar en modo read-only;
- cruzar documentos contra codigo y configuracion;
- identificar contradicciones y documentos desactualizados;
- informar archivo, linea o simbolo, afirmacion y evidencia;
- distinguir contradiccion, desactualizacion e incertidumbre;
- sugerir correcciones sin aplicarlas automaticamente;
- declarar lo que no puede verificarse localmente.

Los permisos del auditor deben ser explicitos. No es suficiente declarar
"read-only" en el prompt si se permiten comandos genericos que pueden cambiar
estado, como migraciones, seeders, backups, restores, limpiezas o operaciones
Git destructivas.

## 13. Politica portable de archivo historico

Archivar no equivale a borrar ni a mantener autoridad operativa:

- conservar el contexto historico para auditoria;
- retirar el documento del contexto recurrente de los agentes;
- enlazar el material historico desde el documento vivo cuando sea necesario;
- no duplicar en documentos activos la narrativa completa de una iniciativa;
- registrar fecha y motivo de archivo;
- revisar referencias y consumidores antes de mover un documento;
- preferir una nota de errata separada frente a reescribir historia ya archivada.

La politica concreta debe decidir si el archivo puede consultarse explicitamente
para precedencia historica y quien autoriza una correccion factual. No debe
confundirse "no cargar por defecto" con "prohibir toda consulta".

## 14. Elementos no transferibles de LMS

La metodologia de `C:\laragon\www\lms-test` aporto el modelo, pero no deben
copiarse automaticamente estas decisiones:

- Laravel, PHP, Blade, Eloquent, Pest o helpers de testing especificos;
- SQLite en memoria o cualquier estrategia de base de datos concreta;
- Spatie Permission, Fortify, Stripe o integraciones del producto origen;
- cursos, lecciones, inscripciones, progreso o roles educativos;
- multitenancy, subdominios, conexiones `lms_*` y comandos `tenant:*`;
- los dos frontends Learner/TailAdmin;
- prefijos historicos de tareas como `Hxxx`;
- horarios, binarios, scripts de despliegue y politicas de migracion de LMS;
- reglas que obliguen a tocar una base real sin una aprobacion y un entorno
  seguro.

El proyecto destino debe conservar sus propias reglas de seguridad, datos,
compatibilidad y operacion. La adaptacion debe importar el modelo documental,
no los contratos del producto origen.

## 15. Riesgos y contradicciones conocidas del modelo

La comparacion con LMS identifico riesgos que cualquier adopcion debe revisar:

1. Diferenciar metricas de archivos de test, suites, casos y assertions.
2. Evitar que `docs/ESTADO.md` crezca hasta duplicar la narrativa completa de
   cada spec.
3. Declarar que el estado central prevalece sobre etiquetas historicas de
   `spec.md`, `plan.md` y `tasks.md`.
4. Definir si el archivo historico se consulta explicitamente para precedencia,
   aunque no se cargue como contexto habitual.
5. Restringir comandos del auditor a operaciones verificablemente seguras.
6. Restringir tambien el agente implementador a su `tasks.md` y a operaciones
   aprobadas.
7. Anadir una puerta explicita entre aprobar tasks y autorizar implementacion.
8. Diferenciar codigo implementado, migracion validada, migracion aplicada y
   despliegue operativo.
9. Mantener una unica plantilla o declarar cual comando local tiene precedencia
   sobre templates genericos.

Estas observaciones son criterios de diseno del arnes, no hallazgos que deban
corregirse automaticamente en otros proyectos.

## 16. Guia de adopcion en otro legacy

Para llevar este arnes a un proyecto existente:

1. Inventariar sus documentos y clasificar fuentes vivas, normativas, historicas
   y funcionales.
2. Identificar la Constitution vigente o declarar que no existe y crear una
   propuesta separada de su ratificacion.
3. Identificar o crear `AGENTS.md` sin copiar reglas especificas de otro dominio.
4. Definir `docs/ESTADO.md` como tablero general, incluyendo zonas y registro de
   specs.
5. Crear los documentos canonicos solo para areas que el proyecto pueda
   respaldar con evidencia.
6. Adaptar `spec.md`, `plan.md` y `tasks.md` al stack y a los riesgos reales.
7. Adaptar el agente implementador con permisos y bloqueos propios del proyecto.
8. Configurar el auditor read-only con comandos seguros del proyecto.
9. Ejecutar una primera auditoria documental antes de declarar el arnes listo.
10. Registrar bloqueos, decisiones y validaciones no ejecutadas.
11. Confirmar que el archivo historico no se convierta en una fuente normativa
    duplicada.

## 17. Historial de revisiones del arnes

| Fecha | Cambio | Motivo | Referencia |
|---|---|---|---|
| 2026-09-30 | Instalacion de skills, Context7, flujo SpecKit y estructura documental | Crear el arnes inicial de Sugycom | Commit `0ff2a1f` |
| 2026-09-30 | Abstraccion portable de gobierno documental a partir de LMS | Separar reglas transferibles de decisiones especificas de producto | Revision actual |
| 2026-09-30 | Adaptacion del agente implementador | Ejecutar specs por fases sin copiar reglas especificas de LMS ni permitir operaciones destructivas por defecto | `.opencode/agents/implement.md` |
| 2026-09-30 | Cierre de `gestion-documental` | Resolver T001-T003 y dejar el arnes operativo con incertidumbres generales no bloqueantes | `docs/ESTADO.md` |
