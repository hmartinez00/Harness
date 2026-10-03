# Generar `legacy-baseline.md` a partir de la auditoría, la propuesta de gobernanza y el repositorio

## Objetivo

Genera un documento `legacy-baseline.md` en la raíz del repositorio que establezca el **baseline verificable del sistema legado antes de ratificar `AGENTS.md` y `.specify/memory/constitution.md`**.

El baseline debe servir como puente entre:

1. la auditoría existente del repositorio;
2. la propuesta de gobernanza existente;
3. la evidencia que pueda verificarse directamente en el repositorio y en el entorno local;
4. las decisiones o validaciones que todavía requieran información del equipo.

El resultado **no debe modificar código, configuración, base de datos ni comportamiento del sistema**.

---

# Fuentes obligatorias

Debes utilizar como fuentes principales:

* `legacy-audit.md`
* `governance-proposal.md`
* el repositorio completo
* configuración y artefactos existentes del proyecto que puedan verificarse localmente

Si existen otros documentos relevantes en el repositorio, puedes consultarlos como evidencia complementaria, pero **no sustituyas ni contradigas silenciosamente** los dos documentos principales.

La auditoría ya distingue entre:

* `Hecho observado`
* `Inferencia`
* `Documentación existente`
* `Validación pendiente`

Conserva esa distinción durante todo el análisis.

---

# Regla fundamental

No conviertas automáticamente:

* comportamiento existente → requisito permanente;
* riesgo identificado → decisión de arquitectura;
* recomendación de la auditoría → regla de gobernanza;
* inferencia → hecho;
* ausencia de evidencia → inexistencia.

El objetivo es determinar qué está:

* confirmado por evidencia del repositorio;
* confirmado por inspección del entorno local;
* inferido pero no confirmado;
* pendiente de validación externa o decisión del equipo.

Cuando una afirmación de `legacy-audit.md` pueda comprobarse en el repositorio, **verifícala directamente**.

Cuando no pueda comprobarse localmente, conserva la afirmación como pendiente y explica qué información falta.

---

# Restricciones operativas

## NO modificar

No modifiques:

* código fuente;
* rutas;
* controladores;
* modelos;
* migraciones;
* configuración;
* `.env`;
* base de datos;
* permisos;
* archivos de `storage`;
* dependencias;
* lockfiles;
* scripts;
* scheduler;
* infraestructura;
* archivos de configuración de producción.

No ejecutes comandos destructivos.

No ejecutes:

* migraciones sobre una base de datos real;
* comandos de restauración;
* comandos de backup que puedan modificar o sobrescribir datos;
* comandos de limpieza;
* comandos de borrado;
* comandos que modifiquen permisos;
* comandos que puedan alterar el entorno.

Si para verificar algo necesitas una operación potencialmente destructiva, **no la ejecutes**. Regístrala como `REQUIRES DECISION` o `REQUIRES TEAM INFO`.

---

# Protección de información sensible

No copies al `legacy-baseline.md`:

* contraseñas;
* tokens;
* API keys;
* secretos;
* credenciales SSH;
* valores reales de `.env`;
* contenido de backups SQL;
* información sensible encontrada en logs;
* datos personales innecesarios.

Puedes documentar la existencia de dichos elementos y su ubicación relativa, pero no revelar sus valores.

---

# Qué debes investigar

Usa el repositorio y el entorno local para verificar, en la medida posible, los nueve grupos de validación identificados en `legacy-audit.md`.

## 1. Base de datos de producción

Determina qué puede verificarse localmente sobre:

* motor de base de datos;
* versión;
* configuración disponible;
* conexión configurada;
* migraciones;
* nombres de tablas;
* relaciones relevantes;
* compatibilidad esperada;
* mecanismos de backup/restauración.

No asumas que SQLite representa producción.

Si no puede determinarse el motor/versiones reales de producción, marca:

`REQUIRES TEAM INFO`

e indica exactamente qué información debe proporcionar el equipo.

---

## 2. Roles y permisos

Verifica en el código:

* middleware `auth`;
* middleware `can:*`;
* permisos definidos;
* roles;
* Form Requests;
* políticas si existen;
* exclusiones o excepciones;
* rutas LMS;
* ownership/filtering de proyectos;
* cualquier comportamiento especial relacionado con autorización.

Distingue entre:

* autorización explícitamente definida;
* autorización inferida por código;
* comportamiento no cubierto por tests;
* comportamiento que requiere validación funcional.

No declares que una autorización es correcta o incorrecta únicamente porque exista o falte.

---

## 3. Rutas y superficies públicas

Verifica:

* `routes/web.php`;
* cualquier otro archivo de rutas;
* nombres de rutas;
* rutas públicas;
* presentaciones;
* endpoints JSON usados por AJAX;
* redirects;
* URLs que parezcan formar parte de contratos históricos.

Determina cuáles parecen ser superficies que podrían tener consumidores externos o enlaces históricos.

No inventes consumidores externos.

Cuando el uso externo no pueda demostrarse desde el repositorio, marca:

`REQUIRES TEAM INFO`

---

## 4. Storage y archivos

Inspecciona la estructura relevante de:

* `storage/app`;
* `storage/app/public`;
* uploads;
* documentos;
* imágenes;
* KML;
* XLSX;
* PDFs;
* backups;
* logs;
* cualquier otro archivo relevante.

Determina:

* qué código escribe estos archivos;
* qué código los lee;
* qué rutas los sirven;
* si existen referencias públicas;
* si parecen fixtures;
* si parecen datos operativos;
* si parecen backups;
* si su eliminación/movimiento tendría impacto potencial.

No copies contenido sensible al baseline.

---

## 5. Scheduler y tareas programadas

Verifica:

* `routes/console.php`;
* comandos;
* scheduler;
* cron/documentación;
* duplicidades;
* zonas horarias;
* frecuencia;
* dependencias externas;
* backups;
* tareas de mantenimiento.

Determina qué está realmente definido en código y qué solamente está documentado.

Si no puede determinarse cuál configuración representa producción, marca:

`REQUIRES TEAM INFO`.

---

## 6. Dependencias del sistema operativo

Busca referencias a herramientas externas como:

* `mysqldump`;
* `mysql`;
* `rsync`;
* `zip`;
* `rm`;
* `ssh`;
* `scp`;
* `sshpass`;
* `git`;
* cualquier otro binario invocado desde PHP, Artisan, scripts o configuración.

Para cada dependencia relevante determina:

* dónde se utiliza;
* para qué;
* si existe fallback;
* si parece requerida en producción;
* si depende de Unix/Linux;
* si entra en conflicto potencial con el entorno local Windows/XAMPP.

No reemplaces ninguna de estas dependencias. Solo documenta el estado.

---

## 7. Integraciones externas

Verifica las integraciones identificadas en la auditoría:

* Telegram;
* correo;
* mapas;
* Excel;
* PDF;
* presentaciones;
* URLs externas;
* Colab;
* cualquier API o servicio externo encontrado en el código.

Para cada integración identifica:

* código responsable;
* configuración requerida;
* entradas;
* salidas;
* contratos observables;
* credenciales requeridas, sin revelar valores;
* posibles consumidores;
* tests existentes.

Distingue claramente entre:

`CONFIRMED CONTRACT`

y

`REQUIRES TEAM INFO`.

---

## 8. Versiones efectivas y tests

Verifica:

* `composer.json`;
* `composer.lock`;
* `package.json`;
* lockfile npm correspondiente;
* versión de PHP disponible;
* versión de Node/npm disponible si puede comprobarse sin modificar nada;
* framework;
* paquetes relevantes;
* configuración de testing;
* Pest/PHPUnit;
* tests existentes.

Si es seguro ejecutar los tests, puedes ejecutarlos.

No modifiques archivos para hacerlos pasar.

Registra:

* qué tests fueron ejecutados;
* resultado;
* duración si es relevante;
* errores;
* qué áreas continúan sin cobertura.

Si decides no ejecutar tests, indícalo explícitamente.

---

## 9. Contratos históricos y comportamiento observable

Busca evidencia de:

* nombres de rutas;
* nombres de permisos;
* tablas;
* columnas;
* relaciones;
* formatos Excel;
* formatos PDF;
* nombres/rutas de storage;
* comandos;
* scheduler;
* filtros por usuario;
* endpoints JSON;
* integraciones;
* presentaciones públicas;
* redirects;
* parámetros esperados.

Clasifica cada elemento según su nivel de evidencia.

No declares que algo es un contrato externo simplemente porque exista.

---

# Clasificación obligatoria

Cada hallazgo relevante debe clasificarse usando una de estas categorías:

### `CONFIRMED`

Existe evidencia directa suficiente en el repositorio o entorno local.

### `INFERRED`

Existe evidencia parcial que permite una inferencia razonable, pero no una confirmación completa.

### `REQUIRES TEAM INFO`

La confirmación depende de información que no puede obtenerse del repositorio/entorno local.

### `REQUIRES DECISION`

Existe suficiente información técnica para identificar una cuestión, pero se necesita una decisión explícita del equipo.

### `CONFLICT`

La documentación y el repositorio presentan información contradictoria.

Cuando exista `CONFLICT`, documenta ambas evidencias. No decidas silenciosamente cuál es correcta.

---

# Clasificación de comportamiento

Además de las categorías anteriores, cuando sea útil clasifica el comportamiento existente como:

* `PRESERVE`
* `CHARACTERIZE`
* `CHANGE`
* `UNKNOWN`

## `PRESERVE`

Existe evidencia suficiente de que representa un contrato técnico/funcional que debe tratarse como compatible durante la modernización.

## `CHARACTERIZE`

El comportamiento existe, pero no está suficientemente probado o documentado y debe caracterizarse antes de modificarlo.

## `CHANGE`

Existe evidencia suficiente de que se trata de un problema conocido cuya corrección ya está respaldada por la propuesta de gobernanza o por una decisión explícita.

No significa que debas implementar el cambio ahora.

## `UNKNOWN`

No existe evidencia suficiente para determinar su naturaleza.

---

# Estructura obligatoria de `legacy-baseline.md`

Genera el documento con esta estructura.

```markdown
# Legacy Baseline

## 1. Purpose

## 2. Scope and Evidence

## 3. Baseline Status

### 3.1 Confirmed Facts
### 3.2 Inferences
### 3.3 Requires Team Information
### 3.4 Requires Decisions
### 3.5 Conflicts

## 4. Compatibility Baseline

### 4.1 Routes and Named Routes
### 4.2 Authentication and Authorization
### 4.3 Database Schema and Data
### 4.4 Storage and Files
### 4.5 Imports and Exports
### 4.6 JSON/AJAX Interfaces
### 4.7 External Integrations
### 4.8 Scheduler and Commands
### 4.9 Public Presentations and Other Public Surfaces

## 5. Behavioral Characterization

### 5.1 Behaviors to Preserve
### 5.2 Behaviors Requiring Characterization
### 5.3 Known Problematic Behaviors
### 5.4 Behaviors Whose Intended Semantics Are Unknown

## 6. Operational Baseline

### 6.1 Runtime
### 6.2 Database
### 6.3 Filesystem and Storage
### 6.4 System Binaries
### 6.5 Scheduler
### 6.6 External Services
### 6.7 Testing
### 6.8 Deployment

## 7. Security and Data Protection Baseline

## 8. Modernization Constraints

## 9. Validation Matrix

## 10. Open Decisions

## 11. Risks Not Yet Resolved

## 12. Recommended Next Step
```

Puedes agregar subsecciones cuando sean necesarias, pero no elimines las secciones anteriores.

---

# Reglas específicas para `Compatibility Baseline`

Esta sección es especialmente importante.

No listes simplemente todos los elementos existentes.

Para cada contrato relevante utiliza una tabla como:

| Elemento            | Evidencia                 | Estado             | Tratamiento  |
| ------------------- | ------------------------- | ------------------ | ------------ |
| `example.route`     | `routes/web.php`          | CONFIRMED          | PRESERVE     |
| `permission.name`   | middleware + config       | CONFIRMED          | PRESERVE     |
| filtro de proyectos | Controller X              | INFERRED           | CHARACTERIZE |
| URL externa         | no verificable localmente | REQUIRES TEAM INFO | UNKNOWN      |

El objetivo es que posteriormente `AGENTS.md` y la Constitution puedan basarse en esta sección sin tener que reinterpretar toda la auditoría.

---

# Reglas específicas para Seguridad

Documenta los riesgos encontrados en `legacy-audit.md` solamente cuando también puedas relacionarlos con evidencia verificable.

Entre otros, presta atención a:

* `APP_DEBUG`;
* backups accesibles;
* archivos sensibles en storage público;
* `dd()`;
* TLS deshabilitado;
* `sshpass`;
* `StrictHostKeyChecking=false`;
* secretos en argumentos de procesos;
* construcción de comandos shell;
* autorización LMS;
* logs;
* credenciales.

Pero no conviertas automáticamente estos hallazgos en reglas de implementación.

El baseline debe registrar el **estado actual y la evidencia**, mientras que la Constitution decidirá posteriormente los principios normativos.

---

# Reglas específicas para Modernization Constraints

Distingue cuidadosamente entre:

### Compatibility constraints

Aquello que debe tratarse como contrato hasta que exista evidencia o decisión que permita cambiarlo.

### Technical debt

Problemas existentes que no deben convertirse automáticamente en restricciones.

### Security constraints

Condiciones que pueden justificar romper compatibilidad si existe una decisión de seguridad aprobada.

### Unknowns

Elementos cuyo comportamiento o importancia no puede determinarse.

La existencia de un comportamiento legado no significa por sí sola que deba preservarse indefinidamente.

---

# Validation Matrix

Construye una matriz que permita saber qué está confirmado antes de ratificar la gobernanza.

Usa al menos estas columnas:

| Área | Pregunta | Evidencia disponible | Estado | Acción requerida | Prioridad |
| ---- | -------- | -------------------- | ------ | ---------------- | --------- |

Incluye como mínimo las áreas:

* production database;
* roles/permissions;
* public routes;
* storage;
* scheduler;
* system binaries;
* integrations;
* effective versions;
* tests;
* historical contracts.

Para `REQUIRES TEAM INFO`, formula una pregunta concreta que el equipo pueda responder.

Ejemplo:

> ¿Cuál es el motor y versión de la base de datos utilizada actualmente en producción?

No escribas preguntas genéricas como "validar producción".

---

# Open Decisions

Esta sección debe contener únicamente cuestiones que realmente requieran una decisión humana.

Ejemplos de categorías:

* si un comportamiento legado debe preservarse;
* si una ruta histórica es contractual;
* si una excepción de autorización es intencional;
* cuál es el scheduler oficial;
* qué integración representa el contrato real;
* qué archivos de storage son operativos;
* cuándo una corrección de seguridad puede romper compatibilidad.

No incluy aquí problemas que puedan resolverse simplemente inspeccionando el repositorio.

---

# Reglas de calidad

Antes de finalizar:

1. Comprueba que cada afirmación importante tenga evidencia.
2. No presentes inferencias como hechos.
3. No presentes recomendaciones como decisiones tomadas.
4. No presentes deuda técnica como requisito.
5. No conviertas toda conducta existente en contrato.
6. No inventes información de producción.
7. No inventes consumidores externos.
8. No inventes roles, permisos ni flujos.
9. No copies secretos.
10. No modifiques el sistema.

Cuando cites evidencia, utiliza referencias precisas al archivo y, cuando sea útil, líneas o símbolos concretos.

Ejemplo:

```text
Evidence:
- routes/web.php → route `projects.index`
- app/Http/Controllers/ProjectController.php → `index()`
- app/Models/Project.php → relationship `...`
```

---

# Relación con los documentos existentes

Al finalizar, comprueba explícitamente si `legacy-baseline.md`:

* confirma hallazgos de `legacy-audit.md`;
* refina hallazgos;
* contradice algún hallazgo;
* resuelve alguna `Validación pendiente`;
* agrega nuevos hechos relevantes;
* identifica decisiones todavía pendientes.

No edites `legacy-audit.md` ni `governance-proposal.md`.

Si encuentras una contradicción, regístrala en `## 3.5 Conflicts`.

---

# Regla sobre AGENTS.md y Constitution

NO generes ni modifiques:

* `AGENTS.md`;
* `.specify/memory/constitution.md`.

El objetivo de esta tarea es exclusivamente producir:

```text
legacy-baseline.md
```

Estos documentos se generarán posteriormente a partir del baseline validado.

---

# Procedimiento

Sigue este orden:

1. Lee `legacy-audit.md`.
2. Lee `governance-proposal.md`.
3. Inspecciona la estructura del repositorio.
4. Verifica las afirmaciones de la auditoría contra el código/configuración/documentación.
5. Inspecciona los puntos de validación pendientes.
6. Ejecuta únicamente verificaciones seguras y no destructivas cuando aporten evidencia.
7. Clasifica los hallazgos.
8. Construye el baseline de compatibilidad.
9. Construye la matriz de validación.
10. Identifica decisiones abiertas.
11. Genera `legacy-baseline.md`.
12. Revisa el documento completo para asegurar que no contenga secretos ni afirmaciones no respaldadas.
13. No modifiques ningún otro archivo.

---

# Resultado esperado

Al terminar debes reportar únicamente:

1. que `legacy-baseline.md` fue generado;
2. qué validaciones importantes quedaron resueltas;
3. qué elementos permanecen como `REQUIRES TEAM INFO`;
4. qué elementos permanecen como `REQUIRES DECISION`;
5. cualquier `CONFLICT` detectado;
6. qué archivos fueron modificados.

Si no fue posible verificar algo, dilo explícitamente.

No implementes ninguna corrección durante esta tarea.
