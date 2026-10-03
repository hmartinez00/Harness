Configura en OpenCode todas las skills incluidas en la sección `SKILLS:` del
archivo `laravel_harness/external.txt`.

El inventario es la fuente de verdad para el alcance de esta tarea. Lee el
archivo directamente al iniciar; no uses una lista de skills copiada en este
prompt, no infieras skills desde la sección `MCPs:` y no omitas ninguna entrada
de `SKILLS:`.

Realiza la configuración en este proyecto, no globalmente, salvo que te lo
autorice expresamente. Inspecciona el entorno, elabora primero un plan completo
para todas las skills inventariadas y después ejecuta los pasos seguros del
plan.

## Procedimiento

1. Lee `laravel_harness/external.txt` y extrae todas las entradas bajo
   `SKILLS:` hasta el siguiente encabezado de sección o el final del archivo.
   Registra literalmente cada entrada, detecta entradas vacías o duplicadas e
   interpreta si cada una es un comando de instalación, una referencia a un
   repositorio o una skill identificada por nombre. Si una entrada contiene un
   comando para instalar varias skills, incluye cada skill resultante en el
   alcance. Trata cambios en este archivo como trabajo del usuario: no lo
   edites.
2. Inspecciona las instrucciones del proyecto y las skills/configuración de
   OpenCode existentes. Determina el alcance correcto (local al proyecto),
   conserva las skills y ajustes existentes e identifica duplicados, conflictos
   o cambios sobre skills ya instaladas.
3. Para cada entrada, determina qué instala y desde qué origen. Consulta la
   documentación oficial y vigente de la herramienta de instalación, OpenCode y
   el proveedor de la skill. No asumas que cualquier repositorio o comando
   instala una skill compatible con OpenCode; no inventes opciones ni
   capacidades.
4. Comprueba que los orígenes sean oficiales o confiables y revisa qué código e
   instrucciones se instalarían antes de ejecutarlos. Identifica permisos,
   comandos, accesos a archivos, herramientas externas y otros riesgos
   relevantes. No ejecutes scripts remotos ni instales dependencias globalmente
   sin autorización.
5. **Antes de modificar archivos o instalar skills, presenta un plan que cubra
   cada entrada de la lista.** Incluye una tabla con: entrada del inventario,
   skills incluidas, origen, alcance/ruta de instalación propuesta, archivos
   que cambiarían, validación y bloqueos/riesgos. Añade el orden de trabajo y
   cómo tratarás skills ya instaladas o entradas no compatibles. No empieces la
   ejecución hasta haber presentado el plan. Si se requiere una decisión del
   usuario, detente y solicítala antes de continuar.
6. Tras el plan, instala solo las skills seguras y suficientemente
   documentadas, usando el mecanismo vigente y el alcance local confirmado.
   Evita reemplazar o actualizar una skill existente sin identificar el cambio
   y su impacto. No modifiques `laravel_harness/external.txt`.
7. Si una skill requiere credenciales, permisos especiales, acceso global o
   una decisión pendiente, detente en ese paso e indica exactamente qué falta.
   No escribas secretos en archivos versionados ni expongas sus valores.
8. Valida cada instalación individualmente: confirma que los archivos de la
   skill existen en la ubicación prevista, revisa sus instrucciones y usa el
   comando vigente de la herramienta para listar o comprobar skills cuando esté
   disponible. Distingue entre instalación detectada y skill efectivamente
   reconocida por OpenCode; no declares éxito basándote solo en la salida del
   instalador.
9. Revisa el diff y el estado de Git. No hagas commit.

## Si no es posible completar la configuración

No crees archivos especulativos ni ejecutes comandos de procedencia dudosa. Si
una entrada no identifica una skill instalable, no es compatible con OpenCode,
su origen no es confiable o su instalación no puede verificarse, registra esa
entrada como fallida, omitida o bloqueada con el motivo. Continúa con otras
skills solo si los pasos son independientes y seguros. Si el inventario está
vacío o no se puede interpretar con certeza, detente y reporta el problema; no
deduzcas skills desde otras secciones.

## Informe final

Resume:
- las entradas de `SKILLS:` encontradas y el plan elaborado para cada una;
- las skills instaladas y el estado individual (reconocida, instalada pero no
  reconocida, fallida, omitida o bloqueada);
- el origen, el alcance y las rutas de instalación;
- los archivos modificados;
- las validaciones y sus resultados;
- las dependencias instaladas, si las hubo;
- los permisos o acciones pendientes, sin exponer secretos;
- cualquier riesgo, limitación o decisión necesaria.
