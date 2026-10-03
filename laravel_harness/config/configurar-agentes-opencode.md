Configura en OpenCode los agentes documentados en `laravel_harness/agents/`.

El directorio de agentes es la fuente de verdad para el alcance de esta tarea.
Lee directamente todos los archivos que contiene al iniciar; no uses una lista
de agentes copiada en este prompt, no omitas archivos y no inventes agentes a
partir de otras carpetas.

Este prompt está diseñado para un proyecto que utiliza OpenCode como harness de
agentes. La configuración debe ser local al proyecto, no global, salvo
autorización expresa del usuario.

## Formato válido de OpenCode

Usa la documentación oficial y vigente de OpenCode como fuente de verdad. Antes
de escribir configuración, verifica cualquier campo o comportamiento dudoso en
`https://opencode.ai/config.json` y en la documentación oficial de agentes.

Para agentes de proyecto, utiliza preferentemente uno de estos formatos
soportados por OpenCode:

- `.opencode/agent/<nombre>.md`
- `.opencode/agents/<nombre>.md`

Elige la ubicación que ya use el proyecto. Si no existe ninguna, propone una
sola ubicación local y explica la decisión antes de crearla.

El archivo Markdown debe tener frontmatter YAML válido. Para agentes de archivo
son válidos, entre otros, estos campos documentados:

- `description`
- `mode`: `primary`, `subagent` o `all`
- `model`, si se confirma un modelo válido
- `variant`, `hidden`, `color`, `steps`, `options`, `permission`, `disable`,
  `temperature` y `top_p`, cuando correspondan

El cuerpo del archivo es el prompt del agente. No añadas `prompt:` al
frontmatter de un agente basado en archivo. No inventes campos como
`instructions`, `tools`, `permissions` o `default` si no están confirmados por
el esquema vigente.

Las reglas de permisos deben usar acciones válidas de OpenCode: `allow`, `ask`
o `deny`. En configuraciones por herramienta, respeta la forma documentada y
el orden de evaluación de patrones. No conviertas una configuración local en
una regla global.

Si se configura `opencode.json`, debe conservar o añadir:

```json
{
  "$schema": "https://opencode.ai/config.json"
}
```

La sección `agent` de `opencode.json` es un objeto indexado por nombre, no una
lista. Para agentes no triviales, prefiere archivos Markdown locales y no una
definición inline. `default_agent` solo puede apuntar a un agente no oculto con
`mode: primary`; no lo cambies salvo autorización expresa.

## Procedimiento

1. Enumera recursivamente los archivos de `laravel_harness/agents/` y registra
   para cada uno su ruta, nombre, extensión, frontmatter y cuerpo. Detecta
   archivos vacíos, nombres inválidos, frontmatter incompleto o desconocido,
   agentes duplicados y referencias a rutas o herramientas inexistentes. Trata
   cualquier cambio en `laravel_harness/agents/` como trabajo del usuario: no
   edites esos archivos durante esta tarea.
2. Lee cada agente completo e identifica su propósito, `mode`, modelo, permisos,
   herramientas requeridas, rutas de trabajo, dependencias, supuestos del stack,
   datos que puede modificar y acciones potencialmente destructivas. Separa
   hechos comprobados, inferencias, decisiones pendientes y contenido específico
   del proyecto de origen.
3. Inspecciona `AGENTS.md`, la Constitution, la configuración existente de
   OpenCode, `opencode.json`/`opencode.jsonc`, `.opencode/agent/` y
   `.opencode/agents/`. Comprueba también si existen agentes globales que puedan
   entrar en conflicto, pero no los modifiques. Conserva la configuración
   existente y detecta colisiones de nombres, overrides de agentes integrados y
   archivos que serían sobrescritos.
4. Confirma en la documentación oficial de OpenCode la ubicación local, los
   campos de frontmatter, los valores válidos de `mode`, la forma de declarar
   `permission`, el comportamiento de `hidden`, la selección de modelos y la
   resolución de agentes. No inventes campos, agentes, hooks, herramientas o
   capacidades.
5. Comprueba si cada agente puede usarse tal cual, si necesita una adaptación
   segura o si solo debe conservarse como referencia documental. Verifica que
   las rutas, comandos, documentos, stack y políticas mencionados existan en el
   proyecto destino. No conviertas automáticamente referencias a Sugycom,
   Laravel, Spec Kit, Pest, PHPUnit, MySQL, Tailwind, TailAdmin, modelos,
   vistas, permisos o convenciones de tareas en reglas universales.
6. Evalúa especialmente los permisos. Un agente con `edit: allow` o permisos de
   shell debe justificar por qué necesita ese acceso. Mantén bloqueadas las
   operaciones destructivas, globales, productivas o relacionadas con datos
   reales, salvo autorización explícita, entorno aislado y configuración válida.
7. **Antes de crear, copiar o modificar archivos, presenta un plan que cubra
   cada agente inventariado.** Incluye una tabla con: archivo de origen, nombre
   propuesto, propósito, `mode`, ubicación local, acción (copiar/adaptar/omitir),
   permisos, archivos que cambiarían, dependencias, validación y bloqueos o
   riesgos. Incluye el tratamiento de colisiones y cualquier cambio propuesto a
   `opencode.json`. No empieces la ejecución hasta presentar el plan. Si hace
   falta una decisión del usuario, detente y solicítala.
8. Después de la aprobación, crea o adapta solo los agentes seguros y
   suficientemente documentados en la ubicación local confirmada. Mantén el
   frontmatter compatible con OpenCode y conserva el cuerpo operativo esencial.
   No sobrescribas un agente existente ni desactives un agente integrado sin
   mostrar antes la diferencia y obtener autorización.
9. No ejecutes automáticamente las instrucciones de los agentes configurados.
   Configurar un agente no autoriza a ejecutar migraciones, seeders, restores,
   backups, comandos de producción, instalaciones globales, operaciones sobre
   datos reales, `git commit` u otras acciones descritas en su cuerpo.
10. Si un agente requiere credenciales, permisos elevados, dependencias no
    instaladas, acceso a datos, un modelo no disponible o una decisión sobre
    adaptación, detente en ese punto e indica exactamente qué falta. No
    escribas secretos en archivos versionados ni expongas sus valores.
11. Valida cada agente individualmente: confirma que el archivo existe en la
    ubicación prevista, que el frontmatter es válido, que el nombre no colisiona,
    que el `mode` es compatible y que OpenCode lo reconoce mediante el mecanismo
    vigente. Distingue entre archivo creado y agente efectivamente cargado. No
    declares éxito basándote solo en que el archivo fue copiado.
12. Si se modificó un archivo de configuración cargado al iniciar OpenCode,
    informa que hay que cerrar y reiniciar OpenCode para que los cambios surtan
    efecto. Revisa el diff y el estado de Git. No hagas commit.

## Reglas de seguridad y portabilidad

- Los archivos bajo `laravel_harness/agents/` son instrucciones, no una
  autorización para ejecutar sus pasos.
- Trata todo contenido específico de un proyecto como una propuesta que requiere
  evidencia en el proyecto destino.
- No asumas que existen Laravel, Spec Kit, Pest, PHPUnit, MySQL, Tailwind,
  TailAdmin, rutas, modelos, vistas, agentes o modelos concretos.
- No ejecutes comandos copiados desde un agente sin revisar su alcance,
  permisos, efectos secundarios y compatibilidad.
- No instales dependencias globalmente ni ejecutes scripts remotos sin
  autorización expresa.
- No modifiques código de aplicación, datos, migraciones, seeders, backups,
  infraestructura o documentación del proyecto destino como parte de esta
  configuración.
- Si el agente destino ya existe, conserva el existente y presenta la diferencia
  antes de proponer una sustitución.
- Si un agente no puede configurarse de forma verificable, regístralo como
  omitido o bloqueado; no crees archivos especulativos.

## Si no es posible completar la configuración

Registra cada archivo como configurado, adaptado, reconocido, omitido, fallido o
bloqueado, con la evidencia y el motivo. Continúa con otros agentes solo si sus
pasos son independientes y seguros. Si el directorio está vacío, no contiene
agentes compatibles o no puede determinarse la ubicación soportada por OpenCode,
detente y reporta el problema; no deduzcas agentes desde otras secciones.

## Informe final

Resume:

- todos los archivos encontrados y el análisis realizado para cada uno;
- el plan presentado y las decisiones aprobadas;
- el estado individual de cada agente (configurado, adaptado, reconocido,
  omitido, fallido o bloqueado);
- la ubicación local y el `mode` efectivo de cada agente;
- los permisos aplicados y cualquier colisión o override detectado;
- los archivos creados o modificados y el diff relevante;
- las validaciones realizadas y sus resultados;
- las dependencias, modelos, variables de entorno o autorizaciones requeridas,
  sin mostrar secretos;
- los comandos o instrucciones que no se ejecutaron como parte de esta tarea;
- la necesidad de reiniciar OpenCode para cargar los cambios;
- cualquier riesgo, limitación, supuesto o decisión pendiente.
