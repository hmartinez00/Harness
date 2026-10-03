# Prompt: especificar un esquema portable de gestion documental

## Proposito

Genera una especificacion para adaptar al proyecto actual un esquema de gestion
documental que permita encontrar reglas operativas, principios de gobierno,
evidencia del sistema, estado de iniciativas, decisiones, validaciones e
historia sin duplicar fuentes ni confundir documentacion tecnica con contenido
funcional del producto.

Registra la propuesta completa en un paquete de iniciativa con
`spec.md`, `plan.md` y `tasks.md`, siguiendo el flujo y las ubicaciones que ya
use el proyecto. Si no existe una convencion, usa `specs/<feature-name>/` y
propone `gestion-documental` como identificador, salvo conflicto con una
iniciativa existente.

Esta tarea es de especificacion y planificacion. No implementes la estructura,
no edites documentos de gobierno existentes y no modifiques codigo, datos,
configuracion, storage o comportamiento del producto.

## Referencias conceptuales

Usa `specs/gestion-documental/` de Sugycom, si esta disponible, solo como
ejemplo de organizacion y nivel de detalle: contexto, estado actual, escenarios
de usuario, requisitos funcionales, casos limite, entidades, criterios
medibles de exito, supuestos, decisiones, plan tecnico, fases, validaciones,
tareas dependientes y Definition of Done.

No copies sus decisiones como requisitos universales. En particular, no impongas
`AGENTS.md`, una Constitution, `docs/ESTADO.md`, `docs/`, `specs/`,
Spec Kit, OpenCode, Laravel, PHPUnit/Pest, Windows/XAMPP, nombres de estados,
clasificaciones, prefijos de tareas ni ubicaciones de archivo si el proyecto
destino no los usa o no los aprueba.

## Fase 1: Descubrir el proyecto

Antes de redactar, inspecciona las fuentes existentes y relevantes:

1. Reglas para agentes y desarrolladores, como `AGENTS.md` u homonimos.
2. Constitution, politica de contribucion, arquitectura y decisiones ratificadas.
3. Baseline, auditoria, inventario tecnico, riesgos y deuda conocidos.
4. Indices de documentacion, estado del proyecto, backlog y registro de
   iniciativas.
5. Specs, planes, tareas, plantillas, checklists y convenciones de ciclo de
   vida.
6. Configuracion de agentes, comandos, skills y herramientas documentales, solo
   si participan en el proceso vigente.
7. Rutas o mecanismos que sirvan documentacion a usuarios, si existen, para
   distinguirlos de la documentacion interna.
8. Estado de Git y cambios existentes en los archivos que se referencien o
   propongan.

No repitas una auditoria general. Sigue las referencias desde las fuentes
principales y examina codigo/configuracion solo cuando sea necesario para
determinar si una afirmacion documental es cierta o para caracterizar el
impacto de separar documentacion interna de contenido funcional.

Clasifica las afirmaciones relevantes con las categorias que ya utilice el
proyecto. Si no hay convencion, propone una que distinga claramente hechos
confirmados, inferencias, decisiones pendientes, informacion que requiere al
equipo, conflictos y desconocidos. No presentes una inferencia como hecho.

## Fase 2: Delimitar el esquema propuesto

La especificacion debe evaluar y proponer, segun corresponda al proyecto:

- una fuente de reglas operativas y su relacion con los principios de gobierno;
- una Constitution u otra fuente normativa solo si el proyecto la necesita,
  incluyendo precedencia, estado de aprobacion y mecanismo de enmienda;
- fuentes de baseline y auditoria como evidencia, separadas de reglas y estado
  vivo;
- una fuente canonica de estado para iniciativas, fases, validaciones,
  bloqueos, decisiones y backlog, o una alternativa compatible con la estructura
  existente;
- documentacion tecnica organizada por responsabilidades observadas, evitando
  duplicar inventarios y dejando referencias a fuentes mas detalladas;
- specs/planes/tareas por iniciativa, con umbral de uso o una alternativa
  proporcional para cambios pequenos;
- una checklist de cierre y criterios de validacion proporcionales al riesgo;
- una politica de archivo que preserve referencias y deje claro que lo
  historico no es automaticamente estado vigente;
- auditoria documental de solo lectura, solo si el proyecto tiene plataforma y
  permisos capaces de imponer efectivamente esa restriccion;
- integracion con comandos, agentes o herramientas existentes, sin duplicar
  fuentes de verdad ni instalar/configurar herramientas como parte de esta
  especificacion.

Para cada elemento, documenta el problema que resuelve, la fuente o consumidor
actual, la opcion propuesta, las alternativas, el impacto y las decisiones que
requieren aprobacion. Si un componente no aplica, explica por que; no lo agregues
por completar una lista.

## Contenido requerido de `spec.md`

Adapta la plantilla local e incluye como minimo:

- titulo, feature branch o identificador, fecha y estado documental;
- contexto y evidencia del estado documental actual;
- fuentes existentes y responsabilidad de cada una;
- objetivo, alcance y exclusiones;
- historias de usuario priorizadas, con justificacion;
- escenarios de aceptacion verificables;
- requisitos funcionales numerados y criterios medibles de exito;
- decisiones de gobierno documental propuestas, claramente separadas de
  hechos confirmados;
- casos limite: falta de fuentes, duplicacion o contradiccion, informacion
  productiva desconocida, documento historico, contenido sensible y
  documentacion servida por el producto;
- entidades y relaciones documentales, si ayudan a explicar el esquema;
- supuestos y preguntas clasificadas como pendientes;
- compatibilidad: documentos, rutas, automatizaciones, consumidores y
  estructuras existentes que se preservan, caracterizan, cambian o se
  desconocen;
- validaciones y riesgos.

No declares aprobada ninguna politica que todavia no haya sido decidida por el
equipo. No conviertas recomendaciones del baseline, auditoria o documentos
historicos en reglas normativas por inferencia.

## Contenido requerido de `plan.md`

Adapta el contexto tecnico al proyecto real; registra herramientas, lenguajes,
plataformas y validadores unicamente cuando esten confirmados. Incluye:

- resumen de la solucion documental y limites de alcance;
- estructura actual y propuesta de archivos con responsabilidades;
- dependencias entre decisiones y fases;
- tratamiento de fuentes que deben conservarse o requieren migracion;
- proteccion de referencias, enlaces, rutas y consumidores existentes;
- plan para separar documentacion interna de contenido funcional, si aplica;
- seguridad y privacidad de documentos;
- validacion de enlaces, referencias, duplicacion, coherencia y estructura;
- validaciones no aplicables, inseguras o dependientes de decisiones del equipo;
- estrategia de adopcion, transicion, archivo y reversibilidad;
- riesgos, alternativas y decisiones abiertas.

No asumas que el checkout local representa produccion. No propongas renombrar,
mover, borrar o migrar documentos sin caracterizar referencias y obtener las
decisiones necesarias.

## Contenido requerido de `tasks.md`

Desglosa trabajo ejecutable por fases e historias. Cada tarea debe tener
identificador conforme a la convencion local (o proponer una si no existe),
dependencias reales, paths concretos cuando ya puedan conocerse, resultado
esperado y criterio de validacion.

Incluye solo tareas justificadas por la solucion aprobable. Considera, cuando
aplique:

1. confirmar inventario, fuentes canonicas y decisiones bloqueantes;
2. establecer o adaptar estructura e indices documentales;
3. registrar estado, iniciativas, decisiones, validaciones y backlog;
4. definir templates, flujo de especificacion y checklist de cierre;
5. enlazar reglas operativas, baseline/evidencia, documentacion tecnica y
   archivo historico;
6. adaptar agentes o herramientas existentes, si estan dentro del alcance;
7. validar consistencia, referencias, duplicacion, seguridad y separacion de
   contenido funcional;
8. revisar diff/status, actualizar el registro canonico y reportar pendientes.

No marques tareas completadas. No incluyas tests de aplicacion por defecto para
un cambio puramente documental; especifica checks documentales adecuados y
condiciona cualquier prueba de aplicacion a que sea pertinente y segura.

## Limites de seguridad y aprobacion

Durante esta tarea:

- crea o modifica exclusivamente los tres documentos de la iniciativa;
- no edites los documentos que la futura iniciativa propone crear o actualizar;
- no instales herramientas ni dependencias, no actives integraciones ni envies
  datos a servicios externos;
- no ejecutes migraciones, operaciones sobre datos, storage, despliegues,
  comandos destructivos ni tests contra entornos no aislados;
- no copies secretos, credenciales, datos personales innecesarios, backups ni
  logs sensibles a la spec;
- conserva todo cambio preexistente y no sobrescribas decisiones ratificadas.

Si falta una decision indispensable, registra la pregunta concreta y su impacto;
no la resuelvas por cuenta propia. Si las fuentes existentes se contradicen,
documenta el conflicto y la autoridad que debe resolverlo.

## Revision final

Antes de terminar:

1. Comprueba que spec, plan y tasks concuerden y que cada tarea tenga un
   resultado y una validacion trazables.
2. Comprueba que las decisiones de Sugycom se usen como referencia, no como
   requisitos por defecto del proyecto destino.
3. Comprueba que hechos, inferencias, propuestas y decisiones pendientes esten
   diferenciados.
4. Comprueba que los paths citados existan o queden marcados como pendientes.
5. Revisa que no se duplique el estado ni se confunda documentacion interna con
   contenido funcional.
6. Revisa `git diff` y `git status` sin revertir cambios ajenos.
7. No ejecutes implementacion ni operaciones adicionales.

En el reporte final indica los documentos de iniciativa creados o modificados,
la estructura propuesta, las decisiones pendientes, los checks realizados y
los elementos que el proyecto destino tendra que confirmar antes de aprobar la
implementacion.
