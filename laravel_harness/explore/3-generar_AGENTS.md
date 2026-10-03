# Tarea: Generar `AGENTS.md` a partir del baseline del legado

Necesito generar el archivo `AGENTS.md` para este repositorio Laravel legado que está siendo modernizado progresivamente mediante Spec Kit.

## Fuentes obligatorias

Utiliza como fuentes, en este orden:

1. `legacy-baseline.md` — fuente principal para el estado actual verificable del sistema.
2. `legacy-audit.md` — contexto factual adicional.
3. `governance-proposal.md` — propuesta de gobierno y reglas operativas ya planteadas.
4. `.specify/memory/constitution.md` — si ya existe y contiene principios ratificados, respétalos; si todavía no existe, no lo inventes.

Además, inspecciona el repositorio únicamente cuando sea necesario para verificar que una regla propuesta corresponde con su estructura real.

---

## Objetivo

Crear un `AGENTS.md` conciso y operativo que indique a cualquier agente de IA cómo trabajar de forma segura sobre este repositorio legado.

`AGENTS.md` debe responder principalmente:

* cómo debe inspeccionar el sistema antes de modificarlo;
* qué debe considerar contrato de compatibilidad;
* qué cosas no debe modificar de manera incidental;
* qué validaciones debe realizar;
* qué operaciones requieren especial precaución;
* cómo debe tratar los elementos desconocidos;
* cómo debe manejar cambios de seguridad;
* cómo debe reportar limitaciones, supuestos y validaciones no ejecutadas.

No conviertas `AGENTS.md` en documentación arquitectónica del sistema.
El estado detallado del legado pertenece a `legacy-baseline.md`.

---

## Regla fundamental

Distingue estrictamente entre:

* hechos confirmados;
* inferencias;
* información que requiere confirmación del equipo;
* decisiones todavía no tomadas.

No conviertas automáticamente `REQUIRES TEAM INFO`, `REQUIRES DECISION`, `CONFLICT` o `UNKNOWN` del baseline en reglas normativas.

Cuando una cuestión no esté resuelta, la regla para el agente debe ser:

> no asumir; identificar la incertidumbre, evitar cambios irreversibles y solicitar/registrar la decisión correspondiente cuando sea necesaria.

---

## Qué debe contener `AGENTS.md`

Estructura recomendada:

### 1. Propósito y alcance

Explica brevemente que el repositorio es un sistema Laravel legado en modernización incremental y que el agente debe priorizar cambios pequeños, verificables y controlados.

### 2. Regla de inspección previa

Antes de modificar una funcionalidad, el agente debe inspeccionar como mínimo los elementos relevantes del flujo existente.

Según el caso, esto puede incluir:

* rutas;
* middleware;
* controllers;
* Form Requests;
* models;
* migrations;
* policies o mecanismos equivalentes de autorización;
* permisos/roles Spatie;
* views;
* JavaScript/AJAX;
* storage;
* comandos;
* scheduler;
* integraciones externas;
* tests existentes.

No asumir que una única capa representa todo el comportamiento.

### 3. Compatibilidad inicial

Establece como regla operativa que, salvo decisión explícita o riesgo de seguridad:

* nombres de rutas y parámetros;
* autenticación y sesión;
* roles y permisos;
* nombres y estructura de tablas;
* relaciones y claves;
* formatos de importación/exportación;
* formatos de documentos;
* rutas de almacenamiento;
* endpoints AJAX;
* comandos existentes;
* efectos del scheduler;
* integraciones externas;
* presentaciones públicas existentes;

deben tratarse como superficies de compatibilidad inicial.

Aclara que esto significa **preservación inicial**, no conservación indefinida.

### 4. Cambios sobre base de datos

Establece que:

* no se deben modificar destructivamente migrations ya utilizadas en entornos existentes;
* los cambios de esquema deben realizarse mediante nuevas migrations;
* no debe asumirse SQLite como equivalente a producción;
* cualquier cambio de nombres, columnas, relaciones o tipos debe analizar su impacto;
* los datos existentes deben considerarse parte del contrato hasta demostrar lo contrario.

### 5. Autorización y permisos

Indica que el agente debe preservar y verificar los mecanismos existentes de:

* autenticación;
* middleware `auth`;
* middleware `can`;
* Spatie Permission;
* roles;
* permisos;
* ownership cuando exista.

No debe eliminar restricciones ni ampliar acceso simplemente para facilitar una implementación.

Si una operación actualmente parece carecer de autorización adecuada, debe tratarse como una cuestión de seguridad/compatibilidad que requiere caracterización y, cuando corresponda, corrección explícita.

### 6. Storage y archivos

Establece que los archivos existentes no deben considerarse automáticamente públicos, temporales o descartables.

El agente debe:

* evitar eliminar/mover archivos sin justificación;
* verificar quién consume una ruta de almacenamiento antes de cambiarla;
* tratar documentos, imágenes, PDFs, XLSX, CSV, DOCX, ZIP, KML y backups existentes como datos potencialmente relevantes;
* considerar `public/storage` una superficie sensible;
* evitar exponer nuevos archivos sin analizar autorización y propósito.

### 7. Integraciones externas

Telegram, correo, mapas, Git, Python, Colab, APIs y demás integraciones observadas deben tratarse como contratos potenciales.

Antes de modificar una integración, inspeccionar:

* configuración;
* consumidores;
* formatos;
* autenticación;
* mensajes/payloads;
* errores;
* efectos secundarios.

No introducir cambios incompatibles basándose únicamente en una suposición.

### 8. Scheduler, comandos y operaciones del sistema

Tratar comandos, scheduler, backups, scripts y operaciones mediante shell como superficies operativas sensibles.

Antes de modificarlos:

* identificar quién los ejecuta;
* identificar cuándo se ejecutan;
* identificar qué datos afectan;
* identificar sus dependencias;
* revisar posibles efectos sobre producción.

No ejecutar operaciones destructivas sobre datos reales.

No asumir que el scheduler observado localmente representa producción.

### 9. Seguridad

Establece reglas explícitas para:

* no introducir secretos en código;
* no copiar secretos desde `.env`;
* no incluir credenciales en logs, commits o mensajes;
* no desactivar TLS/SSL para resolver problemas de conectividad;
* no ampliar permisos de archivos innecesariamente;
* no introducir nuevos comandos shell inseguros;
* no usar credenciales como argumentos de procesos cuando exista una alternativa segura;
* no desactivar controles de autenticación/autorización para facilitar pruebas.

Los problemas de seguridad conocidos pueden justificar romper compatibilidad, pero el cambio debe ser explícito, delimitado y validado.

### 10. Cambios incrementales

Priorizar:

* cambios pequeños;
* alcance limitado;
* una responsabilidad por cambio;
* reutilización de la estructura existente;
* mínima introducción de nuevas capas/frameworks;
* compatibilidad cuando sea posible.

No aprovechar una tarea pequeña para refactorizar incidentalmente otras partes del sistema.

Los problemas encontrados pero fuera del alcance deben documentarse como deuda o hallazgo, no corregirse automáticamente.

### 11. Tests y validación

Antes de declarar terminado un cambio, el agente debe:

1. revisar `git diff`;
2. revisar `git status`;
3. verificar que no existan modificaciones no relacionadas;
4. ejecutar los tests relevantes cuando exista un entorno seguro;
5. ejecutar build/lint/validaciones relevantes cuando corresponda;
6. revisar rutas/permisos/migrations/storage cuando hayan sido afectados;
7. verificar impactos sobre scheduler, comandos e integraciones cuando corresponda.

Si un test no puede ejecutarse de forma segura, debe indicarse explícitamente y no fingir que fue validado.

Particularmente, no ejecutar tests destructivos contra una base de datos con datos reales o no aislada.

### 12. Tratamiento de incertidumbre

Cuando el baseline marque algo como:

* `REQUIRES TEAM INFO`;
* `REQUIRES DECISION`;
* `CONFLICT`;
* `UNKNOWN`;

el agente debe:

* identificarlo;
* no inventar una respuesta;
* no convertir una inferencia en hecho;
* evitar cambios irreversibles;
* solicitar una decisión si la tarea depende de ello.

### 13. Reporte final del agente

Todo trabajo debe terminar indicando brevemente:

* qué cambió;
* qué no cambió;
* validaciones ejecutadas;
* validaciones no ejecutadas;
* riesgos o incertidumbres detectadas;
* decisiones que quedan pendientes;
* cualquier deuda o hallazgo fuera del alcance.

---

## Reglas que NO deben introducirse

No agregues reglas normativas que no estén sustentadas por las fuentes.

En particular, no inventes:

* una arquitectura nueva;
* una política de API;
* una estrategia de repositorios/services;
* una política de frontend;
* una política de despliegue;
* una base de datos de producción;
* una política de testing inexistente;
* una estrategia de CI/CD;
* reglas de negocio no documentadas.

No conviertas las recomendaciones de modernización en obligaciones automáticas si todavía no fueron ratificadas.

---

## Relación con otros documentos

`AGENTS.md` debe dejar clara esta separación:

* `legacy-audit.md` → evidencia y observaciones del legado.
* `legacy-baseline.md` → baseline verificable y clasificación de incertidumbres.
* `.specify/memory/constitution.md` → principios de gobierno ratificados.
* `AGENTS.md` → instrucciones operativas para agentes.
* backlog/specs/tasks → trabajo concreto de cada iniciativa.

No dupliques innecesariamente el contenido del baseline.

---

## Restricciones de esta tarea

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
* ejecutar comandos destructivos;
* crear archivos adicionales fuera de `AGENTS.md`.

Si `AGENTS.md` ya existe, no lo sobrescribas silenciosamente. Analiza primero su contenido y reemplázalo solamente si corresponde a la tarea solicitada.

---

## Resultado esperado

Genera un único archivo:

`AGENTS.md`

Debe ser:

* claro;
* conciso;
* normativo;
* accionable por un agente de IA;
* específico para este repositorio;
* compatible con una modernización incremental;
* sin convertir decisiones pendientes en hechos;
* sin duplicar el baseline.

Antes de finalizar, realiza una comprobación de consistencia:

1. ¿Cada regla importante está sustentada por el baseline, audit o governance proposal?
2. ¿Alguna incertidumbre fue convertida accidentalmente en una regla?
3. ¿Alguna decisión pendiente fue resuelta silenciosamente?
4. ¿Las reglas de seguridad están expresadas como reglas operativas y no como diagnóstico?
5. ¿El documento permite trabajar sobre el legado sin congelarlo permanentemente?
6. ¿Existe alguna regla que contradiga una decisión ya ratificada en la Constitution, si esta existe?

Si detectas alguna contradicción o ambigüedad, repórtala en lugar de resolverla arbitrariamente.
