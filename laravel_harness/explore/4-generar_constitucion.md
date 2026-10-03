# Tarea: Generar la Constitution del proyecto

Necesito generar `.specify/memory/constitution.md` para este repositorio Laravel legado en proceso de modernización incremental.

La Constitution será el documento normativo de mayor jerarquía del proyecto para gobernar las decisiones de modernización. Debe establecer principios estables de gobierno, no describir nuevamente la arquitectura existente ni convertirse en un backlog técnico.

## Fuentes obligatorias

Utiliza las siguientes fuentes:

1. `governance-proposal.md` — fuente principal para los principios de gobierno propuestos.
2. `legacy-baseline.md` — fuente principal para conocer qué hechos, riesgos, incertidumbres y decisiones pendientes existen realmente.
3. `AGENTS.md` — reglas operativas ya derivadas del baseline y de la propuesta de gobierno.
4. `legacy-audit.md` — únicamente como evidencia técnica complementaria cuando sea necesario.

Si existe una Constitution previa en `.specify/memory/constitution.md`, inspecciónala antes de modificarla.

---

# Objetivo

Crear una Constitution breve, normativa y estable que gobierne la modernización progresiva del sistema legado.

La Constitution debe establecer:

* principios fundamentales;
* prioridades entre principios cuando entren en conflicto;
* reglas para cambios incompatibles;
* requisitos mínimos de verificabilidad;
* reglas de gobierno sobre datos y esquema;
* reglas de seguridad;
* reglas de autorización;
* reglas de operabilidad y reproducibilidad;
* límites de la modernización incremental;
* criterios mínimos de aceptación para cambios relevantes;
* mecanismo para modificar la propia Constitution.

Debe evitar describir detalles que pertenecen al baseline o a `AGENTS.md`.

---

# Principio fundamental de interpretación

La Constitution debe distinguir estrictamente entre:

* **principios normativos**;
* **hechos del sistema legado**;
* **riesgos conocidos**;
* **deuda técnica**;
* **decisiones pendientes**.

No conviertas automáticamente un hallazgo técnico en un principio constitucional.

Por ejemplo:

* “Telegram tiene TLS deshabilitado” es un hecho/riesgo del baseline.
* “Las integraciones externas deben mantener comunicación segura y verificable” puede ser un principio constitucional.

De igual forma:

* “existen dos definiciones de scheduler” es un hecho;
* “el scheduler oficial debe estar explícitamente identificado y ser reproducible” puede ser una regla de gobierno.

---

# Principios que deben evaluarse

La `governance-proposal.md` propone los siguientes principios. Evalúalos y conviértelos en principios constitucionales claros, sin agregar otros arbitrariamente:

## I. Compatibilidad antes de la modernización

La modernización debe preservar inicialmente el comportamiento existente cuando este sea conocido, relevante y seguro.

La compatibilidad no significa conservar indefinidamente todo comportamiento histórico.

Antes de romper una superficie de compatibilidad deben existir:

* caracterización suficiente;
* identificación del impacto;
* decisión explícita;
* estrategia de migración cuando corresponda;
* validación del comportamiento nuevo.

La compatibilidad no debe utilizarse para justificar la permanencia de una vulnerabilidad de seguridad claramente identificada.

## II. Seguridad por defecto

Los cambios deben favorecer:

* protección de secretos;
* autenticación;
* autorización;
* validación de entradas;
* transporte seguro;
* mínimo privilegio;
* protección de archivos y datos;
* ausencia de información sensible en logs;
* operaciones seguras sobre shell y sistemas externos.

Cuando seguridad y compatibilidad entren en conflicto, la protección de datos y seguridad debe prevalecer, pero el cambio debe ser explícito, delimitado y verificable.

## III. Autorización explícita y verificable

Los cambios que afecten acceso a funcionalidades, datos, archivos o acciones administrativas deben preservar o fortalecer la autorización existente.

Debe poder determinarse:

* quién puede ejecutar una acción;
* quién no puede;
* qué registros puede afectar;
* qué archivos puede leer/escribir/eliminar;
* bajo qué rol, permiso o condición de ownership.

No debe eliminarse una restricción simplemente porque dificulte una implementación.

## IV. Cambios verificables y regresión controlada

Los cambios relevantes deben ser verificables.

La estrategia debe favorecer:

* tests relevantes;
* caracterización de comportamiento;
* validaciones reproducibles;
* revisión del diff;
* identificación explícita de validaciones no ejecutadas;
* control de regresiones.

No declarar como validado aquello que no pudo ejecutarse de forma segura.

## V. Datos y esquema como contratos

Los datos existentes y el esquema utilizado por entornos existentes deben tratarse como contratos hasta demostrar lo contrario.

Los cambios deben considerar:

* tablas;
* columnas;
* tipos;
* claves;
* relaciones;
* índices;
* JSON persistido;
* migrations;
* datos existentes;
* consumidores internos y externos conocidos.

Las migrations ya utilizadas no deben modificarse destructivamente como mecanismo de cambio.

Los cambios de esquema deben utilizar mecanismos de migración compatibles.

## VI. Operabilidad reproducible

Una funcionalidad no debe considerarse correctamente modernizada únicamente porque funciona en el entorno local del desarrollador.

Los cambios relevantes deben considerar:

* entorno de ejecución;
* dependencias;
* configuración;
* scheduler;
* workers;
* comandos;
* storage;
* integraciones externas;
* permisos del sistema;
* procesos de despliegue y rollback cuando corresponda.

No asumir que una configuración local representa producción.

## VII. Simplicidad y modernización incremental

La modernización debe reducir riesgo, no introducir complejidad innecesaria.

Priorizar:

* cambios pequeños;
* alcance controlado;
* responsabilidades claras;
* reutilización razonable;
* soluciones simples;
* evolución incremental.

No introducir nuevas arquitecturas, frameworks, APIs, repositorios, services o capas únicamente por preferencia estilística.

Una iniciativa de modernización puede introducir nueva arquitectura cuando exista una necesidad documentada y verificable.

---

# Jerarquía de decisiones

La Constitution debe establecer explícitamente la siguiente jerarquía:

1. Seguridad y protección de datos.
2. Integridad y preservación de datos existentes.
3. Compatibilidad con comportamiento y contratos existentes.
4. Verificabilidad y operabilidad.
5. Simplicidad e incrementalidad.

Esta jerarquía debe utilizarse para resolver conflictos entre principios.

No debe interpretarse como permiso para romper compatibilidad arbitrariamente.

Cuando un principio superior obligue a romper uno inferior, el cambio debe quedar explícitamente documentado y validado.

---

# Estado del legado frente a la Constitution

La Constitution NO debe afirmar como hechos normativos cosas que el baseline todavía clasifica como:

* `REQUIRES TEAM INFO`;
* `REQUIRES DECISION`;
* `CONFLICT`;
* `UNKNOWN`.

En particular, no resolver arbitrariamente:

* motor y topología de producción;
* roles y permisos reales;
* consumidores externos;
* scheduler oficial;
* política definitiva de storage;
* semántica de ownership;
* binarios disponibles en producción;
* estrategia definitiva de compatibilidad frente a correcciones de seguridad.

Cuando una de estas cuestiones sea relevante para una iniciativa, debe existir una decisión explícita antes de convertirla en una regla específica.

---

# Calidad y gates

Define gates constitucionales mínimos para cambios relevantes.

Como mínimo, considerar:

### Compatibilidad

Verificar impacto sobre:

* rutas;
* autenticación;
* permisos;
* datos;
* storage;
* formatos;
* integraciones;
* comandos;
* scheduler.

### Seguridad

Verificar:

* secretos;
* autenticación;
* autorización;
* exposición de datos;
* archivos;
* TLS;
* shell/comandos;
* logs.

### Datos

Verificar:

* migrations;
* esquema;
* relaciones;
* datos existentes;
* rollback o estrategia de recuperación cuando corresponda.

### Verificación

Verificar:

* tests relevantes;
* build cuando corresponda;
* validaciones manuales cuando sean necesarias;
* validaciones no ejecutadas y su motivo.

### Operabilidad

Cuando corresponda, revisar:

* scheduler;
* workers;
* storage;
* configuración;
* dependencias;
* despliegue;
* rollback.

No todos los gates deben aplicarse mecánicamente a cambios triviales. Deben aplicarse proporcionalmente al riesgo y alcance del cambio.

---

# Relación con AGENTS.md

La Constitution tiene precedencia normativa sobre `AGENTS.md`.

Establece explícitamente:

* Constitution → principios y gobierno.
* `AGENTS.md` → instrucciones operativas para agentes.
* `legacy-baseline.md` → estado verificable del sistema.
* `legacy-audit.md` → evidencia y observaciones.
* specs/plans/tasks → alcance de iniciativas concretas.

`AGENTS.md` no puede contradecir la Constitution.

Cuando exista una contradicción, debe prevalecer la Constitution y el conflicto debe hacerse explícito.

---

# Deuda técnica

No conviertas automáticamente la deuda técnica observada en obligaciones constitucionales.

Por ejemplo, la existencia de:

* helpers;
* controllers grandes;
* ausencia de repositories/services;
* cobertura de tests limitada;
* nomenclatura inconsistente;
* acceso dinámico a DB;
* shell commands;
* código comentado;
* ausencia de CI/CD;

no significa que la Constitution deba prohibirlos automáticamente.

La Constitution debe gobernar decisiones futuras; no utilizarse para reescribir retrospectivamente todo el legado.

---

# Modernización y compatibilidad

Establece que toda iniciativa de modernización debe indicar, cuando corresponda:

* comportamiento existente que se preserva;
* comportamiento que se caracteriza;
* comportamiento que cambia;
* contratos afectados;
* riesgos;
* estrategia de migración;
* estrategia de validación.

La modernización no debe confundirse con una reescritura total.

---

# Modificación de la Constitution

Incluye un mecanismo de enmienda.

Toda modificación debe registrar:

* versión;
* principio afectado;
* motivo;
* impacto;
* estrategia de transición cuando corresponda;
* fecha;
* estado de ratificación.

Una nueva decisión concreta de un proyecto no debe modificar silenciosamente la Constitution.

---

# Formato

Genera `.specify/memory/constitution.md`.

Utiliza una estructura apropiada para Spec Kit, incluyendo como mínimo:

* título;
* versión;
* estado;
* principios numerados;
* jerarquía de principios;
* quality gates;
* reglas de gobernanza/enmiendas.

Mantén el documento suficientemente compacto para que un agente pueda consultarlo y aplicarlo durante el trabajo cotidiano.

No dupliques el inventario técnico de `legacy-baseline.md`.

---

# Versión inicial

La Constitution generada debe considerarse:

**Version: 0.1.0**
**Status: DRAFT / PENDING RATIFICATION**

No presentar el documento como ratificado si todavía no existe una aprobación explícita del equipo.

---

# Restricciones de esta tarea

Esta tarea es exclusivamente documental.

NO:

* modificar código;
* modificar configuración;
* modificar migrations;
* modificar base de datos;
* modificar storage;
* instalar dependencias;
* actualizar dependencias;
* ejecutar migraciones;
* ejecutar operaciones destructivas;
* crear archivos adicionales fuera de `.specify/memory/constitution.md`.

Si ya existe una Constitution, no sobrescribirla silenciosamente. Comparar primero su contenido con las fuentes y preservar cualquier principio previamente ratificado salvo que exista una instrucción explícita para modificarlo.

---

# Comprobación final

Antes de finalizar, verifica:

1. ¿Cada principio está sustentado por `governance-proposal.md`?
2. ¿El principio es realmente normativo y no simplemente una descripción del legado?
3. ¿Está respaldado por la realidad descrita en `legacy-baseline.md`?
4. ¿Se convirtió alguna decisión pendiente en una decisión tomada?
5. ¿Se confundió una recomendación técnica con una obligación constitucional?
6. ¿Existe una jerarquía clara para conflictos entre seguridad y compatibilidad?
7. ¿La Constitution permite modernizar progresivamente sin congelar el legado?
8. ¿`AGENTS.md` puede operar bajo esta Constitution sin contradicciones?
9. ¿Las reglas son proporcionales al riesgo y no burocráticas por defecto?
10. ¿El documento puede gobernar futuras iniciativas sin tener que reescribirse para cada feature?

Si alguna respuesta es negativa, corrige el documento antes de finalizar o reporta explícitamente la ambigüedad en lugar de resolverla arbitrariamente.
