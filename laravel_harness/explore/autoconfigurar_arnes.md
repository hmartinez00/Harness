# Prompt: configurar un arnes de agentes para un proyecto legacy

## Objetivo

Configura en este repositorio un arnes de trabajo asistido por agentes, adaptado
al proyecto y a las herramientas que realmente estén disponibles. El arnes debe
aprovechar la exploración ya realizada y el `AGENTS.md` existente para que los
agentes puedan especificar cambios, implementar tareas autorizadas y auditar la
documentación con límites claros.

Este prompt es una fase posterior a la exploración del legado. No repitas la
auditoría general ni vuelvas a generar `AGENTS.md`, salvo que el usuario lo pida
expresamente.

La referencia conceptual es el modelo portable del documento
`docs/archive/arnes-configuracion-agentes-2026-09.md`, especialmente sus
secciones sobre jerarquía documental, quality gates, aprobaciones, evidencia,
auditoría read-only, archivo histórico y adopción. Si esa referencia no existe
en el repositorio destino, aplica las reglas portables de este prompt sin
requerirla.

## Resultado esperado

Deja configurados, en el alcance aprobado, los componentes aplicables de este
arnés:

1. Skills relevantes para el proyecto.
2. Una integración MCP de documentación actualizada (Context7 cuando sea
   compatible y útil).
3. Un flujo local para especificar iniciativas, equivalente en propósito a
   `/config_spec`, que produzca especificación, plan y tareas.
4. Un subagente implementador limitado a las tareas y autorizaciones de la
   iniciativa.
5. Un subagente auditor documental en modo efectivamente read-only.
6. Una estructura mínima de gobierno documental que conecte reglas operativas,
   decisiones, estado, iniciativas, evidencia e historia.

Adapta nombres, ubicaciones, formatos y mecanismos a la plataforma de agentes
presente. No instales todos los componentes de forma mecánica: determina cuáles
son pertinentes, compatibles y seguros; registra como no aplicable o bloqueado
lo que no pueda configurarse. No presentes un componente como operativo si no
fue creado y verificado.

## Precondiciones y fuentes

Antes de escribir:

1. Confirma que la exploración del proyecto terminó y localiza `AGENTS.md`.
   Léelo completo. Si falta, está vacío o contradice materialmente las fuentes,
   detente y reporta el bloqueo; no sustituyas la fase de exploración creando
   reglas sin evidencia.
2. Localiza y lee, si existen, la auditoría, baseline, Constitution, propuesta
   de gobierno, estado del proyecto, documentación técnica y configuración
   actual de agentes/herramientas.
3. Inspecciona los archivos de configuración locales y de proyecto necesarios
   para identificar plataforma, capacidades, skills, MCPs, comandos y subagentes
   ya instalados.
4. Revisa el estado de Git y el contenido de cada archivo que planees tocar.
   Conserva cambios preexistentes y no sobrescribas configuraciones o decisiones
   ratificadas.

Aplica estas distinciones:

- `AGENTS.md` contiene instrucciones operativas existentes y debe respetarse.
- La Constitution, si existe y está ratificada, contiene principios normativos;
  no la edites ni la reinterpretes como parte de esta tarea.
- Auditorías y baselines son evidencia del proyecto, no autorización para
  realizar cambios de runtime.
- Una propuesta o documento histórico no es por sí solo una decisión vigente.
- Mantén separados los hechos confirmados, inferencias, datos que requieren al
  equipo, decisiones pendientes, conflictos y desconocidos.
- Para comportamiento existente, distingue lo que se preserva, caracteriza,
  cambia o permanece desconocido.

Si las fuentes discrepan, conserva el conflicto y señala qué decisión se
necesita. No conviertas una recomendación en regla aprobada, una inferencia en
hecho ni la falta de evidencia en prueba de inexistencia.

## Inventario previo y mapa de cambios

Antes de configurar, presenta un inventario breve que cubra:

- plataforma y formato de configuración de agentes detectados;
- `AGENTS.md` y documentos de gobierno/evidencia disponibles;
- skills y mecanismo de instalación existentes;
- MCPs e integraciones de documentación existentes;
- comandos/workflows de especificación existentes;
- subagentes existentes y sus permisos efectivos;
- duplicados, conflictos, brechas y elementos no verificables.

Después, prepara un mapa de cambios con cada archivo o integración propuesta,
su propósito, alcance (proyecto/usuario/organización), efectos y validación.
Evita duplicar herramientas o crear dos fuentes de verdad. Reutiliza las
configuraciones existentes cuando sean adecuadas.

Procede con cambios locales, reversibles y limitados al repositorio que estén
claramente dentro de este objetivo. Solicita aprobación antes de cualquier
acción que:

- afecte configuración global, cuentas, organización o repositorios ajenos;
- instale o actualice dependencias con efectos fuera del proyecto, incurra en
  costos o requiera permisos elevados;
- envíe datos a servicios externos, active integraciones con credenciales o
  requiera crear/usar secretos;
- cambie runtime, datos, permisos de usuarios, despliegues o sistemas externos;
- sea destructiva, irreversible o incompatible con una decisión ya aprobada.

No solicites ni escribas credenciales en archivos, argumentos, logs o
documentos. Si una herramienta necesita una clave, deja documentado el nombre
del mecanismo seguro esperado (por ejemplo, variable de entorno o gestor de
secretos), nunca su valor.

## Criterios de configuración

### 1. Skills

- Inspecciona qué skills admite la plataforma y cómo se declaran en alcance de
  proyecto.
- Descubre y selecciona solo las skills pertinentes a capacidades observables
  del proyecto. Incluye diseño frontend únicamente si hay una superficie
  frontend relevante.
- Prefiere fuentes oficiales y el mecanismo de instalación soportado. Por
  ejemplo, si el entorno admite el CLI `npx skills` y la skill es pertinente,
  el documento de referencia registra `frontend-design` desde
  `https://github.com/anthropics/skills`.
- Revisa instrucciones, alcance y licencia antes de adoptar una skill. No copies
  skills internas del entorno como si fueran archivos del repositorio.
- Registra las skills instaladas, su origen y cómo actualizarlas; no dupliques
  contenido de upstream innecesariamente.
- Si no hay catálogo o mecanismo seguro disponible, informa el bloqueo y no
  simules la instalación.

### 2. MCP de documentación

- Comprueba si el cliente de agentes detectado soporta MCP y si admite
  configuración remota a nivel de proyecto.
- Cuando sea compatible y pertinente, configura Context7 usando su endpoint
  HTTPS documentado: `https://mcp.context7.com/mcp`.
- Conserva TLS y validación de certificados. No guardes API keys en la
  configuración, repositorio, historial, terminal ni logs.
- Si hace falta una clave, documenta el mecanismo seguro de provisión y marca la
  conexión como pendiente hasta que pueda validarse sin revelar el secreto.
- Verifica la conexión con el comando/herramienta oficial disponible. No
  interpretes una respuesta HTTP aislada como prueba de que MCP funciona.
- Aclara que documentación externa ayuda a consultar herramientas/librerías,
  pero no prueba hechos del repositorio ni decide políticas del proyecto.
- No configures el servicio si la plataforma no lo soporta o su activación
  implica una autorización no otorgada; explica la alternativa y el bloqueo.

### 3. Flujo de especificación

Configura un comando, prompt o flujo nativo equivalente a `/config_spec` que:

- solicite un identificador de iniciativa y su objetivo;
- inspeccione el flujo afectado antes de planificar;
- adapte la inspección al stack, incluyendo las capas pertinentes de rutas,
  middleware, controladores, requests, modelos, esquema, permisos, vistas,
  JavaScript, storage, formatos, integraciones, comandos, scheduler y tests;
- genere artefactos equivalentes a `spec.md`, `plan.md` y `tasks.md` en una
  ubicación existente o acordada, sin imponer `specs/` si el proyecto usa otra;
- separe requisitos, solución propuesta, tareas ejecutables, riesgos,
  compatibilidad, validación, incertidumbres y aprobaciones pendientes;
- no implemente código como efecto implícito de crear la especificación;
- enlace las fuentes canónicas del proyecto en vez de duplicar su contenido.

Si ya existe un flujo equivalente, intégralo o mejóralo sin crear un comando
paralelo. No impongas una plantilla genérica de Spec Kit si contradice el flujo
local o la herramienta disponible.

Si la plataforma detectada es OpenCode y se decide usar Spec Kit, verifica
primero que la versión disponible de `specify` admita esta sintaxis y, solo si
no existe ya una instalación que deba conservarse, inicializa la integración
del repositorio con:

```bash
specify init . --integration opencode
```

Inspecciona los archivos que genere antes de adaptarlos. No vuelvas a ejecutar
la inicialización sobre una configuración existente ni sobrescribas cambios,
integraciones o decisiones locales.

### 4. Subagente implementador

Configura el subagente en la sintaxis nativa de la plataforma. Sus instrucciones
deben establecer que:

- su alcance se limita a las tareas aprobadas de la iniciativa identificada;
- antes de editar, lee `AGENTS.md`, la Constitution ratificada si existe, la
  especificación/plan/tareas y la documentación canónica afectada;
- inspecciona el flujo relevante y preserva inicialmente los contratos
  observados, excepto cambios autorizados o correcciones de seguridad
  delimitadas;
- no resuelve decisiones pendientes ni amplía el alcance por iniciativa propia;
- no ejecuta migraciones reales, seeders, cambios en datos, backups/restores,
  limpiezas, operaciones Git destructivas, despliegues ni cambios externos sin
  autorización explícita y entorno seguro;
- no crea secretos ni los imprime; no desactiva autenticación, autorización,
  TLS o validación para facilitar el trabajo;
- valida con los tests y herramientas pertinentes que sean seguros, e informa
  claramente validaciones ejecutadas, omitidas y bloqueos;
- entrega un resumen de cambios, archivos afectados, riesgos, decisiones
  pendientes y compatibilidad.

No confíes en una etiqueta textual de “read-only” como control de seguridad:
limita los permisos y comandos disponibles cuando la plataforma lo permita.

### 5. Subagente auditor documental

Configura un auditor que:

- no modifique archivos, configuración, datos ni estado Git;
- compare afirmaciones documentales con código/configuración y cite archivo,
  línea o símbolo y evidencia;
- clasifique discrepancias como contradicción, desactualización, incertidumbre
  o dato no verificable;
- preserve las categorías de evidencia e incertidumbre usadas por el proyecto;
- proponga correcciones, pero no las aplique;
- informe explícitamente qué no pudo verificar.

Restringe el conjunto de herramientas del auditor a lecturas y consultas
seguras. No le asignes shell genérico si puede realizar operaciones mutables;
si no es posible limitarlo efectivamente, declara esa limitación y no lo
describas como técnicamente read-only.

### 6. Gobierno documental mínimo

Conecta el arnés a las fuentes que ya existan, conservando la separación de
responsabilidades:

- reglas operativas para agentes;
- principios normativos y estado de ratificación;
- baseline/auditoría como evidencia del legado;
- estado vivo del proyecto e iniciativas;
- specs/planes/tareas por iniciativa;
- validaciones como evidencia;
- archivo histórico como material no vigente por defecto.

Respeta nombres y ubicaciones locales. Si falta una fuente, registra el hueco y
propón una creación solo cuando sea necesaria para que el componente configurado
funcione. No generes automáticamente una Constitution, un baseline, una auditoría
o reglas de negocio en esta tarea.

Configura quality gates proporcionales al riesgo y las áreas afectadas:
rutas/contratos, autenticación y autorización, datos/esquema, archivos/storage,
interfaces y formatos, integraciones, comandos/scheduler, seguridad, pruebas,
build y operación. Indica explícitamente las áreas no aplicables y las que
requieren decisión o información del equipo.

## Adaptación entre plataformas

- Detecta la plataforma por sus archivos y herramientas disponibles; no asumas
  OpenCode, Claude, Copilot, Spec Kit ni otro proveedor.
- Usa la sintaxis y capacidades nativas para skills, MCP, comandos y subagentes.
- No crees configuraciones incompatibles para múltiples plataformas salvo que
  el repositorio las use y el mapa de cambios justifique mantener paridad.
- Mantén las instrucciones portables separadas de los adaptadores específicos
  de plataforma cuando eso evite duplicación o divergencia.
- Si una capacidad no existe, configura la alternativa compatible de menor
  riesgo o deja el componente pendiente con motivo verificable.
- No declares compatibilidad basándote solo en documentación genérica: valida
  los archivos y comandos concretos presentes.

## Aplicación y verificación

Una vez presentado el inventario y el mapa de cambios:

1. Si hay una acción que requiere aprobación según este prompt, detente en esa
   acción y pregunta antes de ejecutarla. Continúa con los cambios locales
   independientes que sí estén autorizados.
2. Aplica cambios pequeños, coherentes y limitados al arnés. No modifiques
   código de aplicación, esquema, datos, storage o comportamiento runtime.
3. Valida la sintaxis de cada archivo de configuración con herramientas
   disponibles y seguras.
4. Valida que los comandos, skills, subagentes y MCP se detecten o carguen
   mediante mecanismos oficiales de la plataforma. No afirmes conexión,
   ejecución o permisos que no hayas probado.
5. Revisa los permisos efectivos del auditor y del implementador; documenta
   límites no aplicables o imposibles de imponer técnicamente.
6. Revisa `git diff` y `git status`. Confirma que los cambios estén limitados al
   arnés y no alteren modificaciones ajenas preexistentes.
7. No ejecutes pruebas de aplicación, migraciones ni operaciones externas salvo
   que sean necesarias, seguras y estén autorizadas.

Si algo falla, conserva el error relevante sin incluir secretos, no lo ocultes
con un estado de éxito y prueba una alternativa segura cuando exista.

## Entrega final

Al finalizar, informa:

- componentes configurados, adaptados, no aplicables o bloqueados;
- archivos creados/modificados y propósito de cada uno;
- skills y MCP instalados/configurados, su alcance y resultado de verificación;
- flujo de especificación y subagentes disponibles, incluidos sus límites
  efectivos;
- validaciones ejecutadas y no ejecutadas, con motivo;
- aprobaciones, secretos o datos del equipo pendientes, sin exponer valores;
- riesgos, conflictos y decisiones abiertas;
- confirmación de que no se modificó el runtime ni se ejecutaron operaciones
  destructivas.

No declares listo el arnés si sus componentes principales no pueden cargarse,
si el auditor puede modificar estado sin control efectivo o si quedan
integraciones presentadas como verificadas sin evidencia.
