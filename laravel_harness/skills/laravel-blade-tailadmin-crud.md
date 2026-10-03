---
name: laravel-blade-tailadmin-crud
description: Use when creating or redesigning Laravel Blade CRUD panels that use TailAdmin, especially show/create/edit forms, reusable x-form/x-panel components, dark mode, authorization, and Pest feature contracts.
compatibility: Laravel 10-12, Blade, Tailwind CSS, TailAdmin, Pest
metadata:
  scope: reusable
  language: es
---

# Laravel Blade + TailAdmin CRUD Design

Skill para diseñar o rediseñar CRUDs administrativos en Laravel con Blade y TailAdmin. La prioridad es producir interfaces coherentes, accesibles y mantenibles sin romper autorización, scoping, persistencia ni contratos existentes.

## Cuándo usarla

Usar esta skill cuando la tarea incluya:

- Rediseñar `show`, `create`, `edit` o un `form.blade.php` de un recurso Laravel.
- Migrar un CRUD desde markup genérico, Breeze o una plantilla TailAdmin inconsistente.
- Sustituir fichas basadas en `toArray()` por una vista curada.
- Extraer componentes reutilizables `x-form.*` o `x-panel.*`.
- Alinear un CRUD con dark mode, autorización por rol/permiso u ownership.
- Crear la suite Pest que define el contrato visual y funcional del CRUD.

No usarla para el frontend público, APIs JSON sin Blade, Livewire/Inertia/React/Vue, ni para un rediseño puramente visual que no toque un CRUD.

## Contexto obligatorio

Antes de editar, inspeccionar el proyecto concreto. No asumir nombres de rutas, componentes, middleware ni permisos.

1. Leer `AGENTS.md`, `README.md` y las convenciones locales.
2. Localizar modelo, migración, controlador, Form Request, rutas y vistas del recurso.
3. Identificar el layout TailAdmin y los componentes Blade existentes.
4. Leer la política de roles, permisos y ownership/scoping.
5. Revisar tests existentes del CRUD y contratos compartidos.
6. Consultar documentación local de diseño si existe; esta skill no sustituye las convenciones del proyecto.
7. Ejecutar `git status` y preservar cambios ajenos.

Si el proyecto usa un sistema de componentes distinto, adaptar las reglas a sus equivalentes en lugar de introducir nombres ficticios.

## Proceso visual antes de codificar

La skill de CRUD no sustituye la exploración visual. Antes de implementar, hacer una pasada breve de diseño inspirada en `frontend-design`:

1. Identificar el trabajo principal del usuario en cada vista: localizar, comparar, inspeccionar, editar o confirmar.
2. Elegir una dirección visual concreta para el recurso: densidad, jerarquía, tratamiento de imágenes, color de estados y comportamiento responsive.
3. Definir un mini sistema de tokens: 4-6 colores funcionales, escala tipográfica, radios, espaciado y reglas de foco.
4. Dibujar un wireframe corto para `index`, `show` y `form` antes de escribir Blade.
5. Revisar el plan contra el contexto del recurso: no usar la misma composición de cards para todos los dominios.
6. Mantener una sola decisión visual memorable por vista; eliminar decoración que no ayude a comprender o actuar.

La identidad existente del panel tiene prioridad. La variedad debe venir de la jerarquía y del tratamiento del contenido, no de romper el sistema TailAdmin.

## Catálogo de templates del proyecto

Antes de escoger una composición, localizar el catálogo local de templates. Los proyectos pueden registrar sus demos y componentes en un documento como `docs/tailadmin-template-catalog.md`.

El catálogo debe indicar, para cada referencia:

- Ruta Blade y ruta HTTP o nombre de ruta.
- Patrón que demuestra: tabla, formulario, badge, avatar, alert, media, chart, card o layout.
- Componentes que realmente se pueden reutilizar.
- Si es demo visual, componente productivo o solo referencia.
- Restricciones conocidas: permisos, datos estáticos, JavaScript, responsive y dark mode.

Usar los templates como biblioteca de patrones, no como bloques para copiar sin revisar. Separar siempre markup, datos, permisos y acciones del demo original.

## `index`: anatomía y patrones

El `index` es una vista de trabajo, no una simple salida de la tabla. Diseñarlo según la tarea principal del recurso.

### Anatomía base

```text
breadcrumb
  → encabezado del recurso + descripción breve + Add New autorizado
  → filtros/búsqueda/orden opcionales
  → toolbar secundaria: resultados, acciones masivas o vista
  → tabla, lista o cards
  → estado vacío, paginación y feedback
```

Reglas generales:

- El título debe nombrar el recurso en lenguaje de usuario, no el nombre de la tabla.
- El CTA `Add New` solo aparece si el usuario puede crear; su URL debe ser una ruta nombrada.
- Mostrar únicamente columnas que ayuden a identificar, comparar o actuar.
- Mantener una columna de acciones con acciones autorizadas y labels accesibles.
- Resolver relaciones, estados, dinero, fechas, imágenes y porcentajes antes de llegar a Blade.
- Eager-load las relaciones usadas en las filas y evitar N+1.
- Declarar columnas mediante `$columns` o la API equivalente del componente de tabla.
- Aplicar scoping antes de paginar, filtrar o contar resultados.
- Preservar paginación y query string cuando se usen filtros.
- Definir estados vacíos y de carga/error de forma explícita cuando el stack lo permita.
- En móvil, permitir scroll horizontal controlado o cambiar a una lista/card; nunca comprimir todas las columnas hasta hacerlas ilegibles.
- Mantener dark mode, foco de teclado, contraste y targets táctiles adecuados.

### Patrones de índice

Elegir uno y justificarlo en el spec:

| Patrón | Usarlo cuando | Elementos mínimos |
|---|---|---|
| Tabla curada | El usuario compara muchos registros | columnas declarativas, acciones, estados, responsive overflow |
| Tabla con identidad | El recurso tiene avatar, portada o título largo | media + título + metadato, `alt`, truncado con acceso al valor completo |
| Lista compacta | Hay pocas columnas y una acción principal | fila semántica, estado, acción, empty state |
| Cards/grid | La imagen o el estado visual domina la decisión | proporción consistente, título, metadata, CTA y alternativa accesible |
| Nested index | El recurso depende de un padre | contexto del padre, parámetro correcto, breadcrumb, scoping y CTA contextual |
| Tabla con filtros | Hay muchos registros o estados | filtros reales en servidor, query persistente, reset y resultado visible |

No convertir todos los índices en cards por estética. Una tabla es correcta cuando la comparación es el trabajo central.

### Backend del índice

El controlador o action debe preparar una fila de presentación estable. Por ejemplo:

```php
$columns = [
    ['key' => 'title', 'label' => 'Title', 'type' => 'image-title'],
    ['key' => 'status', 'label' => 'Status', 'type' => 'badge'],
    ['key' => 'updated_at', 'label' => 'Updated', 'type' => 'date'],
];
```

La forma exacta depende del componente local. La regla es que Blade no decida relaciones, clases de estado, formato monetario, permisos ni URLs complejas.

### Anti-patrones específicos de `index`

- `toArray()` crudo como tabla de producción.
- Todas las columnas de la base de datos visibles por defecto.
- IDs de relaciones en lugar de nombres.
- `1`/`0`, enum crudo o clases CSS calculadas en Blade.
- Acciones visibles sin permiso o acciones que enlazan a URLs inventadas.
- Título heredado de una demo como `Index Table Card`.
- Tabla sin overflow o sin estrategia móvil.
- Paginación aplicada después de cargar todos los registros.
- Consultas N+1 por resolver relaciones dentro de cada fila.
- Datos estáticos del template presentados como datos reales.

### Contrato Pest del `index`

La suite debe comprobar, según aplique:

- título, breadcrumb y CTA `create` autorizado;
- columnas esperadas y ausencia de columnas técnicas;
- nombres de relaciones, badges y formatos visibles;
- URL y permiso de cada acción;
- scoping por rol/tenant y ocultación de acciones;
- filtros, búsqueda, paginación y preservación de query string;
- empty state;
- marcadores del componente de tabla y clases responsive/dark;
- ausencia de `toArray()` crudo, títulos de demo y URLs placeholder.

## Modelo visual objetivo

### Layout

- Las vistas nuevas deben usar el layout TailAdmin establecido por el proyecto, normalmente `@extends('layouts.app')` + `@section('content')`.
- No introducir `x-app-layout` de Breeze en vistas nuevas si el panel usa un layout TailAdmin.
- Todas las páginas deben conservar breadcrumb, título, navegación y responsive behavior del panel.
- Formularios y fichas deben funcionar en claro y oscuro; no usar colores que solo funcionen en un tema.
- Usar rutas nombradas con `route()`; no escribir URLs internas manualmente.

### `show`

Una ficha curada debe tener:

- Hero con título del recurso, media opcional, estado semántico y CTA real de edición.
- Metadatos breves y útiles, no un volcado completo del modelo.
- Una o más tarjetas de detalles con labels legibles, estados formateados y relaciones resueltas.
- Empty states explícitos para contenido opcional.
- Fechas y valores monetarios formateados en el controlador o presenter, no con lógica de negocio en Blade.

Evitar:

- `toArray()` o JSON crudo en la interfaz.
- `x-cruds.show` si es un modal genérico o inerte que no representa el diseño objetivo.
- `$tableRowData` reutilizado como sustituto de una ficha curada.
- Botones que parezcan editables pero no enlacen a la ruta real.
- Mostrar columnas técnicas, secretos, hashes, tokens o campos internos sin una razón explícita.

### `create`, `edit` y `form`

- `create` y `edit` deben compartir un único `form.blade.php` siempre que la estructura sea la misma.
- Usar un wrapper TailAdmin consistente y dark-aware para ambas vistas.
- El `form` debe dividirse en grupos semánticos, no en una lista plana de inputs.
- Las diferencias entre create y edit deben limitarse a action, método HTTP, título y timestamps readonly.
- `edit` debe declarar explícitamente `@method('PATCH')` o el verbo que corresponda.
- Los timestamps readonly se muestran solo cuando existe el modelo.
- El footer debe tener una acción primaria clara y una cancelación hacia el índice.
- Mantener `old()` y errores de validación sin reinyectar secretos o payloads sensibles.

## Componentización

Preferir componentes Blade existentes y ampliar su API antes que duplicar markup.

Componentes habituales:

- `x-form.text-input`, `x-form.textarea`, `x-form.select`, `x-form.number-input`.
- `x-form.checkbox`, `x-form.switch`, `x-form.image-slot`, `x-form.cover-upload`, `x-form.avatar-upload`.
- `x-panel.hero`, `x-panel.details-card`, `x-panel.definition-grid`, `x-panel.status-pill`.
- Componentes de layout, breadcrumb, card y submit bar ya establecidos por el proyecto.

Reglas:

- Un componente transversal nuevo debe vivir en `resources/views/components/` y tener API explícita.
- No duplicar bloques de `x-data` de imágenes dentro de cada CRUD.
- Toda subida de imagen debe usar el componente canónico del proyecto; separar crop/live si el diseño lo requiere.
- No crear componentes genéricos para resolver un único caso si un partial local es suficiente.
- No modificar el HTML de CRUDs no incluidos en el alcance sin una regresión explícita.

## Diagnóstico por recurso

Antes de diseñar, preparar una tabla con:

1. Modelo, tabla, clave primaria y route model binding.
2. Relaciones y FKs que deben ser selects o enlaces.
3. Tipos de columnas: texto, `longText`, enum, boolean, fecha, decimal, JSON, array e imagen.
4. Campos técnicos que no deben aparecer en `show` o deben ser readonly.
5. Ownership y scoping: qué puede leer, crear, editar o borrar cada rol.
6. Estado de `index`, `show`, `form`, `create` y `edit`.
7. Rutas, Form Request, gates/policies y middleware aplicables.
8. Anti-patrones encontrados y contratos de tests afectados.

## Mapeo de campos

- FK o enum: `select` con opciones reales y labels legibles.
- `longText`: `textarea`, nunca input de una línea.
- Boolean: checkbox, switch, chip o label semántico; nunca `1`, `0`, `true` o `false` crudos.
- Imagen: componente de upload/preview del proyecto; validar MIME, tamaño y almacenamiento.
- JSON/array: repeater, editor específico o exclusión documentada; nunca `[object Object]`.
- Fechas: inputs adecuados y formato de presentación consistente.
- Dinero: número validado y formato monetario en la presentación.
- Relaciones: mostrar nombre/título, no solo IDs.
- Secretos: ocultar o excluir por defecto de HTML, sesión, logs y respuestas de validación.

## Backend y autorización

- La lógica de negocio vive en controlador, action, service o dominio; nunca en Blade.
- Validar siempre con Form Request y reforzar reglas de longitud, tipo, enum, existencia y unicidad.
- `authorize()` debe reflejar el acceso real o delegar al mecanismo de autorización vigente.
- Preservar ownership/scoping en `index`, `show`, `create`, `edit`, `store`, `update` y `destroy`.
- No confiar en campos ocultos para seguridad; forzar en servidor los valores derivados del usuario autenticado.
- Comprobar explícitamente acceso a registros ajenos: normalmente 404 por ocultación de existencia o 403 según la política del proyecto.
- No añadir permisos nuevos si el proyecto tiene una decisión explícita de reutilizar un permiso/gate existente.
- No crear rutas nuevas si el comportamiento puede implementarse dentro del resource existente.
- No cambiar migraciones ejecutadas; si falta esquema, crear una migración nueva y actualizar la documentación del modelo.

## Flujo TDD recomendado

### Fase 0: contrato rojo

Crear o actualizar `tests/Feature/<Resource>EditorTest.php` antes de tocar producción. Los tests deben cubrir:

- Hero, details card, status y CTA real en `show`.
- Ausencia de modal genérico o JSON crudo.
- Grupos, componentes y tipos correctos del formulario.
- Coherencia create/edit y método HTTP.
- Persistencia válida e invalidación de payloads.
- Uploads, arrays, relaciones y campos opcionales relevantes.
- Guest, roles sin permiso y registros ajenos.
- Scoping del índice y de los selects.
- Dark mode y marcadores semánticos de componentes.

Evitar aserciones frágiles sobre markup irrelevante. Preferir marcadores estables de componentes, atributos semánticos, rutas nombradas y ausencia de patrones prohibidos.

Antes de testear:

```bash
php artisan config:clear
php artisan route:clear
```

Usar `route:clear` cuando cambien rutas. En uploads, usar requests multipart; no esperar que `postJson()` transporte `UploadedFile`. Incluir todos los campos required para evitar falsos rojos.

### Implementación por fases

Orden recomendado:

1. `index` curado: patrón, columnas, acciones, scoping y estado vacío.
2. `show` curado.
3. `form` compartido con grupos, selects y textareas.
4. Wrapper `edit` y coherencia create/edit.
5. Uploads, JSON y transformaciones.
6. Validación y persistencia.
7. Regresión de autorización, índices y contratos compartidos.

Cerrar cada fase con el filtro Pest del recurso. No avanzar con fallos no diseñados.

### Verde y regresión

Ejecutar:

- Suite específica del recurso.
- Tests de breadcrumb, dark mode, tablas e imágenes afectados.
- Tests de ownership/scoping si el recurso pertenece a un tutor, instructor o tenant.
- Suite completa con la memoria y comandos establecidos por el proyecto.

Los tests de diseño pueden usar un régimen rojo/skipped durante la fase 0 solo si el proyecto lo documenta. Al cerrar la feature, retirar el `beforeEach` de skip y ejecutar la suite normalmente.

## Contratos Pest recomendados

Preferir marcadores como:

```php
$response->assertSee('panel-hero');
$response->assertSee('panel-details-card');
$response->assertSee('form-select');
$response->assertSee('panel-definition');
$response->assertSee(route('resources.edit', $model), false);
$response->assertDontSee('open-profile-info-modal');
```

Para HTML con comillas, `>` o `&`, usar el segundo argumento `false` en `assertSee`/`assertDontSee` cuando se esté comprobando markup literal. Comprobar create/edit por separado:

- Create: no debe tener `_method` ni timestamps readonly.
- Edit: debe tener `_method` y timestamps readonly cuando corresponda.
- Ambos: deben compartir los grupos y el mismo `form.blade.php`.

## Definition of Done

- `index` tiene un patrón justificado, columnas curadas, acciones autorizadas, estado vacío y estrategia responsive.
- `show` curado con Hero, CTA real, detalles y estados semánticos.
- `create` y `edit` comparten formulario y usan el wrapper TailAdmin correcto.
- Todas las FKs/enums editables tienen controles adecuados.
- `longText`, booleanos, arrays, imágenes y fechas tienen representación apropiada.
- No se exponen JSON crudo, secretos, campos técnicos ni modales inertes.
- Validación, persistencia, uploads y scoping se conservan o se prueban explícitamente.
- Guest, roles, permisos y registros ajenos tienen tests.
- Suite específica y regresiones pasan en verde.
- Documentación local actualizada según las reglas del proyecto.
- No se añadieron migraciones, permisos o rutas sin necesidad documentada.
- No se commitea sin autorización explícita cuando la política del proyecto la exige.

## Adaptación a otro proyecto

Al usar esta skill en otro repositorio, sustituir únicamente:

- El layout y los nombres reales de los componentes.
- La matriz de roles, policies, gates y scoping.
- Las rutas y nombres de recursos.
- Los helpers y comandos de test.
- La política de documentación y commits.

No copiar la matriz histórica de recursos del LMS ni sus decisiones de negocio: son contexto del proyecto original, no parte de esta skill.
