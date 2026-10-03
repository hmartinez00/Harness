Configura en OpenCode los comandos documentados en `laravel_harness/commands/`.

El directorio de comandos es la fuente de verdad para el alcance de esta tarea.
Lee directamente todos los archivos Markdown que contiene al iniciar; no uses
una lista de comandos copiada en este prompt, no omitas archivos y no inventes
comandos a partir de otras carpetas.

Realiza la configuración en el proyecto actual, no globalmente, salvo que el
usuario lo autorice expresamente. Inspecciona primero el entorno, elabora un
plan completo y ejecuta únicamente los pasos seguros y aprobados.

## Procedimiento

1. Enumera recursivamente los archivos de `laravel_harness/commands/` y registra
   para cada uno su ruta, nombre, extensión y contenido de frontmatter. Detecta
   archivos vacíos, duplicados, nombres inválidos, frontmatter incompleto y
   referencias a rutas o herramientas que no existan. Trata cualquier cambio
   en `laravel_harness/commands/` como trabajo del usuario: no edites esos
   archivos durante esta tarea.
2. Lee cada comando completo e identifica su propósito, agente declarado,
   herramientas requeridas, rutas de destino, comandos de shell, dependencias,
   permisos, supuestos del stack y acciones potencialmente destructivas. Separa
   hechos comprobados, inferencias, decisiones pendientes y contenido
   específico del proyecto de origen.
3. Inspecciona las instrucciones del proyecto (`AGENTS.md`, Constitution y
   configuración existente de OpenCode), así como `.opencode/commands/` y
   cualquier otra ubicación local de comandos soportada por la versión vigente
   de OpenCode. Determina la ubicación local correcta sin asumir que una ruta
   documentada sigue siendo válida. Conserva los comandos existentes y detecta
   colisiones de nombres, comandos duplicados o archivos que serían
   sobrescritos.
4. Consulta la documentación oficial y vigente de OpenCode para confirmar el
   formato de comandos, el frontmatter admitido, la ubicación local, la
   resolución de variables y la forma de seleccionar agentes. No inventes
   campos, agentes, hooks, permisos ni capacidades. Si la documentación no
   confirma una configuración, márcala como bloqueada o requiere decisión.
5. Para cada comando, determina si puede instalarse tal cual, si necesita una
   adaptación segura o si solo debe conservarse como referencia documental.
   Comprueba que las rutas y convenciones específicas del proyecto destino sean
   reales antes de proponer una adaptación. No conviertas automáticamente
   nombres de proyecto, rutas Laravel, comandos de test, prefijos de tareas,
   modelos, vistas o políticas en reglas universales.
6. **Antes de crear, copiar o modificar archivos, presenta un plan que cubra
   cada archivo inventariado.** Incluye una tabla con: archivo de origen,
   nombre del comando, propósito, agente, ubicación local propuesta, acción
   (copiar/adaptar/omitir), archivos que cambiarían, dependencias, validación y
   bloqueos o riesgos. Incluye el orden de trabajo y el tratamiento de
   colisiones. No empieces la ejecución hasta presentar el plan. Si hace falta
   una decisión del usuario, detente y solicítala.
7. Después de la aprobación, crea o adapta solo los comandos seguros y
   suficientemente documentados en la ubicación local confirmada. Mantén el
   frontmatter válido y conserva el contenido operativo esencial. No copies
   referencias específicas del proyecto de origen sin validarlas. No sobrescribas
   un comando existente sin mostrar antes la diferencia y obtener autorización.
8. No ejecutes automáticamente los comandos configurados ni sus instrucciones
   de shell, migraciones, restores, seeders, commits, instalaciones globales,
   operaciones sobre datos reales o cambios de infraestructura. La configuración
   de un comando no equivale a ejecutar su flujo.
9. Si un comando requiere credenciales, permisos elevados, dependencias no
   instaladas, acceso a datos, una herramienta no disponible o una decisión
   sobre adaptación, detente en ese punto e indica exactamente qué falta. No
   escribas secretos en archivos versionados ni expongas sus valores.
10. Valida cada comando individualmente: confirma que el archivo existe en la
    ubicación prevista, que su frontmatter puede ser interpretado, que el nombre
    no colisiona y que OpenCode lo reconoce mediante el mecanismo vigente,
    cuando este exista. Distingue entre archivo creado y comando efectivamente
    reconocido. No declares éxito basándote solo en que el archivo fue copiado.
11. Revisa el diff y el estado de Git. No hagas commit y no modifiques
    `laravel_harness/commands/` ni `laravel_harness/config/` salvo que el usuario
    lo solicite explícitamente.

## Reglas de seguridad y portabilidad

- Los archivos bajo `laravel_harness/commands/` son instrucciones, no una
  autorización para ejecutar sus pasos.
- Trata todo contenido específico de un proyecto como una propuesta que requiere
  evidencia en el proyecto destino.
- No asumas que existen Laravel, Spec Kit, Pest, PHPUnit, MySQL, Tailwind,
  TailAdmin, rutas, modelos, vistas o agentes concretos.
- No ejecutes comandos copiados desde un archivo sin revisar su alcance,
  permisos, efectos secundarios y compatibilidad.
- No instales dependencias globalmente ni ejecutes scripts remotos sin
  autorización expresa.
- No modifiques datos, migraciones, seeders, backups, infraestructura o código
  de aplicación como parte de esta configuración.
- Si el destino ya contiene un comando equivalente, conserva el existente y
  presenta la diferencia antes de proponer una sustitución.
- Si un comando no puede configurarse de forma verificable, consérvalo como
  omitido o bloqueado en el informe; no crees archivos especulativos.

## Si no es posible completar la configuración

Registra cada archivo como configurado, adaptado, omitido, fallido o bloqueado,
con la evidencia y el motivo. Continúa con otros comandos solo si sus pasos son
independientes y seguros. Si el directorio está vacío, no contiene archivos
compatibles o no puede determinarse la ubicación soportada por OpenCode,
detente y reporta el problema; no deduzcas comandos desde otras secciones.

## Informe final

Resume:

- todos los archivos encontrados y el análisis realizado para cada uno;
- el plan presentado y las decisiones aprobadas;
- el estado individual de cada comando (configurado, adaptado, reconocido,
  omitido, fallido o bloqueado);
- la ubicación local de cada comando y cualquier colisión detectada;
- los archivos creados o modificados y el diff relevante;
- las validaciones realizadas y sus resultados;
- las dependencias, agentes, permisos o variables requeridos, sin mostrar
  secretos;
- los comandos que no se ejecutaron como parte de esta tarea;
- cualquier riesgo, limitación, supuesto o decisión pendiente.
