Configura en OpenCode todos los servidores MCP incluidos en la sección `MCPs:`
del archivo `laravel_harness/external.txt`.

El inventario es la fuente de verdad para el alcance de esta tarea. Lee el
archivo directamente al iniciar; no uses una lista de URLs copiada en este
prompt, no infieras recursos MCP desde la sección `SKILLS:` y no omitas ninguna
entrada de `MCPs:`.

Realiza la configuración en este proyecto, no globalmente, salvo que te lo
autorice expresamente. Inspecciona el entorno, elabora primero un plan completo
para todos los MCP inventariados y después ejecuta los pasos seguros del plan.

## Procedimiento

1. Lee `laravel_harness/external.txt` y extrae todas las entradas bajo
   `MCPs:` hasta el siguiente encabezado de sección o el final del archivo.
   Registra literalmente cada URL y comprueba que ninguna entrada esté vacía,
   duplicada o malformada. Trata cambios en este archivo como trabajo del
   usuario: no lo edites.
2. Inspecciona las instrucciones del proyecto y la configuración existente de
   OpenCode. Determina el alcance correcto (local al proyecto), conserva todos
   los servidores y ajustes ya configurados y detecta posibles colisiones de
   nombres o duplicados de URL.
3. Para cada entrada del inventario, determina si es un endpoint MCP remoto, un
   repositorio o paquete que implementa un servidor MCP, documentación u otro
   recurso. No asumas que cualquier URL ofrece MCP.
4. Consulta la documentación oficial y vigente de cada proveedor y la de
   OpenCode para determinar el transporte, el método de conexión, los
   requisitos, las credenciales, los riesgos y la validación compatibles. No
   inventes campos, comandos, capacidades ni instrucciones de instalación.
5. **Antes de modificar configuración o instalar dependencias, presenta un plan
   que cubra cada MCP de la lista.** Incluye una tabla con: URL del inventario,
   nombre de configuración propuesto, tipo de recurso, transporte/método
   propuesto, requisitos o credenciales, cambios previstos, validación y
   bloqueos/riesgos. Añade los archivos que se modificarían, el orden de trabajo
   y cualquier alternativa para entradas no configurables. No empieces la
   ejecución hasta haber presentado el plan. Si se requiere una decisión del
   usuario, detente y solicítala antes de continuar.
6. Tras el plan, ejecuta solo los pasos que sean seguros y estén suficientemente
   documentados. Comprueba que cada origen sea oficial o confiable antes de
   ejecutar código o instalar dependencias. No instales herramientas
   globalmente ni ejecutes scripts remotos sin autorización.
7. Si hacen falta credenciales, no las solicites ni las escribas en archivos
   versionados. Usa únicamente referencias a variables de entorno o al
   mecanismo seguro soportado por OpenCode. Si el usuario debe configurar un
   secreto o conceder autorización, detente en ese paso e indica exactamente
   qué falta, sin exponer valores.
8. Añade o modifica solo la configuración necesaria. No sobrescribas ni
   reformatees ajustes ajenos, y no cambies `laravel_harness/external.txt`.
9. Valida cada MCP de forma individual con los comandos vigentes de OpenCode y,
   cuando sea posible, confirma que conecta. Informa claramente de los que
   quedaron configurados, conectados, fallidos, omitidos o bloqueados. No
   declares éxito basándote solo en que el archivo de configuración exista.
10. Revisa el diff y el estado de Git. No hagas commit.

## Si no es posible completar la configuración

No crees configuración especulativa para una entrada que no ofrezca un servidor
MCP verificable, cuya documentación no permita determinar la configuración o
que requiera una decisión, credencial o autorización pendiente. Continúa con
otros MCP solo si sus pasos son independientes y seguros. Registra la entrada
bloqueada y explica qué verificaste, qué falta y qué dato o autorización se
necesita. Si el inventario está vacío o no se puede interpretar con certeza,
detente y reporta el problema; no deduzcas URLs desde otras secciones.

## Informe final

Resume:
- la lista de MCPs encontrada en `external.txt` y el plan preparado para cada
  uno;
- el estado individual de cada servidor (configurado, conectado, fallido,
  omitido o bloqueado);
- los archivos modificados, sin incluir `external.txt` si no fue alterado;
- los transportes y fuentes documentales utilizados;
- las validaciones realizadas y sus resultados;
- las dependencias instaladas, si las hubo;
- las variables de entorno requeridas, sin mostrar sus valores;
- cualquier limitación o acción pendiente.
