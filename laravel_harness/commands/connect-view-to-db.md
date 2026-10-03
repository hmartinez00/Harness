---
description: Conecta una vista pública del frontend Learner para renderizar datos desde la BD, reemplazando datos hardcodeados. Incluye diagnóstico, spec/TDD, esquema, modelos, seeder, contrato controlador, ruta paramétrica, vista Blade, tests y docs. Basado en las 8 specs del proyecto que conectaron vistas públicas a la BD.
agent: build
---

# Flujo Connect-View-to-DB — Conectar una vista pública a datos reales de la BD

Ejecuta este flujo paso a paso para reemplazar datos hardcodeados en una vista pública del frontend
Learner por datos provenientes de la base de datos. El flujo replica el patrón validado en las 8 specs
del proyecto: `home-sections-from-db`, `home-categories-from-db`, `course-catalog-from-db`,
`course-catalog-filters`, `course-details-dynamic`, `instructor-profile`, `blog-details-dynamic` y
`lesson-player-from-db`.

---

## Paso 0 — Recopilar contexto de la vista a conectar

Pregunta al usuario:

1. **Ruta/vista objetivo** (ej: `/about`, `/pricing`, `/contact`, `/events`)
2. **Qué está hardcodeado**: arrays PHP en `LandingController`, HTML estático en la vista, o ambos
3. **Si existe spec previo** que ya hizo algo similar (cruzar con la lista de specs en `specs/`)
4. **Alcance**: ¿solo reemplazar datos, o también añadir funcionalidad (filtros, paginación, comentarios)?

Explora el codebase para entender el estado actual:

```
routes/web.php              → ruta actual, nombre, grupo LandingController
app/Http/Controllers/
  LandingController.php     → método de la vista, arrays hardcodeados
resources/views/learner/
  {vista}.blade.php         → qué datos usa, qué sección hardcodeada
docs/modelo-de-datos.md     → tablas existentes, columnas, modelos, roadmap (sec. 7)
AGENTS.md                   → features completados, hardcodes restantes
specs/                      → specs existentes similares para copiar patrones
tests/Feature/              → tests existentes, helpers en tests/Pest.php
```

---

## Paso 1 — Crear spec + tests TDD (integrado con Speckit)

Sigue el flujo completo del comando `config_spec.md` para crear la documentación y los tests:

### 1.1 Crear directorio y spec

```
specs/{feature-name}/spec.md
specs/{feature-name}/plan.md
specs/{feature-name}/tasks.md
```

**Prefix de tareas**: revisar `specs/` existentes para evitar colisión de rangos `Hxxx`.

### 1.2 Tests TDD en rojo por diseño

Crea `tests/Feature/{NombreTest}.php` con tests que fallen porque rutas/modelos/vistas aún no existen.
Patrón típico para vistas públicas:

```php
<?php
// Tests de contrato: la vista debe contener datos del seeded post/curso/etc
it('renders the view at the correct url', function () {
    $this->get(route('route.name'))->assertOk()->assertViewIs('learner.vista');
});

it('returns 404 for unknown slug', function () {
    $this->get(route('route.name', 'slug-inexistente'))->assertNotFound();
});

it('does not render null fields', function () {
    // Si un campo es null, no debe aparecer su contenedor HTML
    $this->get(route('route.name'))->assertDontSee('Campo que puede ser null');
});

it('has working links from cards to detail pages', function () {
    // Cards en catálogo/home deben enlazar a la ruta paramétrica
    $this->get(route('home'))->assertSee(route('route.name', $slug));
});

// Tests de post/comentario si aplica
it('creates a record on POST', function () {
    postJson(route('store.name', $slug), [/* valid data */])->assertRedirect();
    $this->assertDatabaseHas('table', ['field' => 'value']);
});
```

Añade helpers en `tests/Pest.php` si los datos de prueba son compartidos (createTestPost, etc.).

### 1.3 Confirmar tests rojos

```bash
php artisan config:clear && php artisan route:clear
php artisan test --filter=NombreTest
```

Todos los tests deben fallar por diseño (Route not defined / Class not found / View not found).

### 1.4 Commit spec

```bash
git add specs/{feature-name}/ tests/Feature/{NombreTest}.php tests/Pest.php
git commit -m "spec({feature-name}): documentacion y tests TDD rojos para {descripcion}"
```

---

## Paso 2 — Migraciones aditivas

> **Regla: las migraciones son aditivas.** Nunca reescribas o modifiques una migración existente que
> ya podría haberse ejecutado en otros entornos.

### 2.1 Verificar necesidad

Cruza lo que la vista necesita con lo que ya existe en `docs/modelo-de-datos.md`:

- ¿La tabla ya existe? → ¿tiene las columnas necesarias?
- ¿Necesita columnas nuevas? → Crea migración aditiva (nullable por defecto).
- ¿Necesita una tabla nueva? → Crea migración completa con FK + índice.
- ¿Ya existe modelo para la tabla? → Solo necesitas actualizar `$fillable`.

### 2.2 Convenciones de migración

```php
// Para columnas nuevas en tabla existente:
Schema::table('posts', function (Blueprint $table) {
    $table->longText('content')->nullable()->after('excerpt');
    $table->string('category')->nullable()->after('excerpt');
    $table->json('tags')->nullable()->after('category');
});

// Para tablas nuevas:
Schema::create('comments', function (Blueprint $table) {
    $table->id();
    $table->foreignId('post_id')->constrained()->cascadeOnDelete();
    $table->string('name');
    $table->string('email');
    $table->text('content');
    $table->foreignId('parent_id')->nullable()->constrained('comments')->cascadeOnDelete();
    $table->unsignedInteger('likes')->default(0);
    $table->timestamps();
    $table->index('post_id');
});
```

### 2.3 Política nivel 5

Los campos sin dato real en producción se **ocultan** en la vista con `@if`, nunca se rellenan con
placeholder. Documentar esta decisión en el spec (sección de Decisiones).

---

## Paso 3 — Modelos Eloquent

### 3.1 Modelo nuevo (tabla nueva)

```php
<?php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Comment extends Model
{
    protected $fillable = ['post_id', 'name', 'email', 'website', 'content', 'parent_id', 'likes'];

    public function post() { return $this->belongsTo(Post::class); }
    public function replies() { return $this->hasMany(self::class, 'parent_id'); }
    public function parent() { return $this->belongsTo(self::class, 'parent_id'); }
}
```

### 3.2 Modelo existente extendido

```php
// Añadir a $fillable:
protected $fillable = ['title', ..., 'content', 'category', 'tags', 'reading_time'];

// Añadir casts si son arrays o fechas:
protected $casts = ['tags' => 'array', 'published_at' => 'datetime'];

// Añadir relaciones:
public function comments() { return $this->hasMany(Comment::class)->whereNull('parent_id'); }

// Añadir scope publicado (si aplica):
public function scopePublished($query) { return $query->where('status', 'published'); }
```

### 3.3 Relación pivotal (curso → instructor en courses table)

Si la tabla ya tiene la FK (ej: `courses.user_id` → instructor), **no crear tabla pivote nueva**.
Verificar la relación existente en el modelo antes.

---

## Paso 4 — Seeder determinista

> **Regla: idempotencia.** Los seeders usan `updateOrInsert`/`updateOrCreate` para no duplicar
> datos si se ejecutan varias veces. Para datos nuevos con secuencia, crear una sección separada
> en `LmsTablesSeeder.php`.

### 4.1 Convenciones del proyecto

```php
// Sección nueva en LmsTablesSeeder.php, al final:

// --- Sección N: {Descripción} ---
$posts = [
    ['slug' => 'post-one', 'title' => 'Post One', 'content' => '<h2 id="intro">...</h2>', ...],
    ['slug' => 'post-two', 'title' => 'Post Two', ...],
];
foreach ($posts as $data) {
    Post::updateOrCreate(
        ['slug' => $data['slug']],  // criterio de identidad
        $data                         // valores a crear/actualizar
    );
}
// Fechas escalonadas para tests:
$posts[0]->update(['published_at' => now()->subMonths(2), 'created_at' => now()->subMonths(2)]);
$posts[1]->update(['published_at' => now()->subMonth(), 'created_at' => now()->subMonth()]);
```

### 4.2 Datos de prueba determinísticos

- Sin Faker: todos los valores son literales inline.
- Fechas: `now()->subMonths(N)` para escalonar.
- Slugs: únicos y predecibles (para usar en tests como `/blog-details/post-one`).
- Contenido HTML: usar `h2 id` alineados a `toc_items` si hay TOC.

### 4.3 Enriquecer datos existentes

Para tablas ya sembradas (posts, cursos, etc.), usar `updateOrCreate` sobre el slug, no
`DB::table()->insert()` (que duplicaría). `created_at` se iguala a `published_at` cuando aplique
para que las fechas sean consistentes.

---

## Paso 5 — Contrato en el controlador

> **Regla: fuente única = BD.** El controlador construye un array "contrato" con forma exacta.
> La vista consume solo claves de ese array. Nunca hay datos hardcodeados en la vista.

### 5.1 Estructura del contrato

```php
public function blogDetails(Post $post)
{
    // 404 si no está publicado
    abort_unless($post->status === 'published', 404);

    // Construir array contrato
    $details = [
        'title'         => $post->title,
        'author'        => $post->author_name,
        'author_avatar' => $post->author_avatar ?? asset('assets/img/person/default-profile.png'),
        'date'          => \Carbon\Carbon::parse($post->published_at ?? $post->created_at)->format('F j, Y'),
        'reading_time'  => $post->reading_time,        // null-ocultado en vista
        'category'      => $post->category,             // null-ocultado en vista
        'tags'          => $post->tags ?? [],            // null-ocultado en vista
        'comment_count' => $post->comments()->count(),
        'image'         => $post->image,
        'toc_items'     => $post->toc_items ?? [],       // null-ocultado en vista
        'content'       => $post->content,
        'share_url'     => url("/blog-details/{$post->slug}"),
    ];

    $comments = $post->comments()->with('replies')->latest()->get();

    return view('learner.blog-details', compact('details', 'comments'));
}
```

### 5.2 Helper mapper (cuando la tarjeta se repite en home + listado)

```php
// Método privado: mapea un modelo Eloquent → array que la vista espera
private function blogPostItem($post): array
{
    return [
        'title'     => Str::limit($post->title, 80),
        'url'       => route('blog.details', $post->slug),
        'date_day'  => \Carbon\Carbon::parse($post->published_at ?? $post->created_at)->format('d'),
        'date_month'=> \Carbon\Carbon::parse($post->published_at ?? $post->created_at)->format('F'),
        'author'    => $post->author_name,
        'category'  => $post->category ?? 'General',
        'image'     => asset($post->image),
    ];
}
```

### 5.3 Eager loading (evitar N+1)

```php
// En listados: eager-load relaciones que la tarjeta necesita
$courses = Course::with(['category', 'user'])
    ->where('status', 'published')
    ->get()
    ->map(fn ($course) => $this->courseCardItem($course));
```

### 5.4 Agregaciones en vivo (portables DB-agnostic)

```php
// En PHP, no en SQL (evita whereRaw/strftime que fallan en SQLite):
$students = $course->enrollments()->count();
$avg = $course->reviews()->avg('rating');  // Eloquent avg es portable
$rating = round($avg, 1);
```

### 5.5 Paginación

```php
// skip() + paginate(): el offset del paginator SUMA al skip previo
$posts = Post::published()
    ->orderByDesc('published_at')
    ->skip(5)           // excluir los 5 del hero
    ->paginate(9)
    ->withQueryString();
// Page 2 → offset = 5 + 9 = 14 (consistente, no un bug)
```

---

## Paso 6 — Ruta paramétrica + cambio atómico

### 6.1 Ruta para vista de detalle

```php
// routes/web.php — dentro del grupo LandingController
Route::get('/blog-details/{post:slug}', 'blogDetails')->name('blog.details');
Route::post('/blog-details/{post:slug}/comment', 'storeComment')->name('blog.comment.store');
```

**Sin route-model-binding global** si los slugs colisionan entre tablas (ej: `lesson.slug` no es único).
En su lugar, resolver manualmente:

```php
$course = Course::where('slug', $slug)->firstOrFail();  // slugs de curso sí son únicos
$lesson = $course->sections->flatMap->lessons->firstWhere('slug', $lessonSlug);
abort_unless($lesson, 404);
```

### 6.2 Cambio atómico (evita Missing required parameter)

> **Gotcha recurrente:** cambiar la ruta de un detalle sin actualizar las tarjetas/cards/header
> que enlazan a ella causa `Missing required parameter` en TODAS las vistas del frontend.

**Cambios que DEBEN hacerse en el mismo commit:**
1. La ruta en `routes/web.php`
2. Los enlaces en las cards (home, catálogo, listados)
3. El ítem del header dropdown (si existía un enlace estático sin parámetro)

```php
// Antes: <a href="{{ route('blog.details') }}">Blog Details</a>  (ruta sin param → 404)
// Después: <a href="{{ route('blog.details', $post['slug']) }}">Blog Details</a>
// Y el header: eliminar el ítem estático o enlazar al listado, no al detalle
```

---

## Paso 7 — Vista Blade dinámica

### 7.1 Null-ocultado (política nivel 5)

```blade
{{-- Si el campo puede ser null, ocultar TODO el contenedor --}}
@if($details['reading_time'])
    <span class="reading-time">{{ $details['reading_time'] }}</span>
@endif

{{-- NO usar placeholders: --}}
<span>{{ $details['reading_time'] ?? 'N/A' }}</span>  {{-- MAL --}}
```

### 7.2 Breadcrumbs con route()

```blade
{{-- MAL --}}
<nav class="breadcrumbs"><ol>
    <li><a href="index.html">Home</a></li>
    <li class="current">Blog Details</li>
</ol></nav>

{{-- BIEN --}}
<nav class="breadcrumbs"><ol>
    <li><a href="{{ route('home') }}">Home</a></li>
    <li><a href="{{ route('blog') }}">Blog</a></li>
    <li class="current">{{ Str::limit($details['title'], 30) }}</li>
</ol></nav>
```

### 7.3 Links a rutas nombradas con slug

```blade
{{-- MAL --}}
<a href="/blog-details/{{ $post['slug'] }}">

{{-- BIEN --}}
<a href="{{ route('blog.details', $post['slug']) }}">
```

### 7.4 Contenido HTML inyectado con { !! !! }

```blade
{!! $details['content'] !!}
```

> **Riesgo de seguridad:** el contenido `content` viene del seeder (controlado por el admin).
> En producción, sanitizar con `strip_tags()` permitiendo solo `<h2>`, `<p>`, `<ul>`, `<li>`, etc.

### 7.5 Estados vacíos

```blade
@if($comments->isEmpty())
    <div class="empty-state">
        <p>No comments yet. Be the first to comment!</p>
    </div>
@else
    @foreach($comments as $comment)
        ...
    @endforeach
@endif
```

### 7.6 JS vanilla (no Vite, no Alpine)

El frontend público Learner **no carga** el bundle Vite del panel TailAdmin.
Si necesitas interactividad (reply buttons, toggle), usa vanilla JS inline:

```blade
@push('scripts')
<script>
document.addEventListener('DOMContentLoaded', function() {
    var btns = document.querySelectorAll('.reply-btn[data-parent-id]');
    btns.forEach(function(btn) {
        btn.addEventListener('click', function() {
            document.getElementById('comment_parent_id').value = btn.dataset.parentId;
        });
    });
});
</script>
@endpush
```

---

## Paso 8 — Tests: pasar a verde

### 8.1 Ejecutar tests de la feature

```bash
php artisan config:clear && php artisan route:clear
php artisan test --filter=NombreTest
```

### 8.2 Gotchas comunes al pasar a verde

| Problema | Solución |
|---|---|
| `assertSee('>2<')` no encuentra | Usar `assertSee('>2<', false)` — `assertSee` escapa el needle por defecto |
| `Missing required parameter` | Asegurar que cards/header usan `route('nombre', $slug)` (paso 6.2) |
| `Route [X] not defined` | Ejecutar `php artisan route:clear` (cache stale) |
| Test de paginación falla | Verificar el offset: `skip(N)->paginate(M)` → page 2 = offset N+M |
| `assertSee` en HTML muy largo es lento | Es normal (~25s/test); en verde cada test corre en ~3s |

### 8.3 Contrato actualizado

Si la feature ya existía con ruta estática (ej: `/blog-details` sin slug), actualizar el contrato
en `tests/Feature/PublicRoutesTest.php` para que apunte a la nueva ruta parametrizada:

```php
// Antes:
$response = $this->get('/blog-details');
// Después:
$response = $this->get('/blog-details/sed-ut-perspiciatis-unde-omnis'); // post sembrado
```

---

## Paso 9 — Docs + commit

### 9.1 Actualizar docs (artefacto obligatorio)

#### `AGENTS.md`

- Añadir bloque **"Feature completado — \`{feature-name}\` (✅)"** después del último feature.
  Incluir: descripción, especificación, suite de tests (tests en verde, total suite),
  implementación (migraciones, modelos, controlador, rutas, vistas, seeder), gotchas.
- Actualizar tabla de cobertura de tests: añadir fila con la nueva suite.
- Actualizar conteos en: intro de cobertura (Pest 4), "Completado recientemente", hardcodes restantes.

#### `docs/modelo-de-datos.md`

- Si hay tablas nuevas: crear sección `1.N {tabla} — Estado: ✅` con esquema, modelo, vistas servidas.
- Si hay columnas nuevas: actualizar la sección de la tabla existente.
- Actualizar **sección 6** (matriz resumen): añadir filas y actualizar columnas "Vistas públicas".
- Actualizar **sección 7** (roadmap): añadir subsección `7.N Spec {feature-name} ✅`.
- Actualizar **sección 7.0** (precedencia): añadir fila con tablas que modifica.
- Actualizar fecha "Última verificación".

#### `README.md`

- Actualizar fila de la tabla de vistas (ruta, descripción dinámica).
- Actualizar lista de rutas públicas.
- Actualizar "hardcodes restantes".

#### `docs/rutas-y-accesos.md`

- Actualizar grupo A (landing público) con la nueva ruta parametrizada.

### 9.2 Commit spec (rojo)

```bash
git commit -m "spec({feature-name}): documentacion y tests TDD rojos para {descripcion}"
```

### 9.3 Commit final (verde)

```bash
git add -A
git commit -m "feat({feature-name}): {descripcion corta}"
```

Si la rama está adelantada de `main`, sincronizar con:
```bash
git checkout main && git merge --ff-only dev
```

---

## Cheat-sheet de gotchas documentadas (8 specs)

| Gotcha | Specs | Impacto |
|---|---|---|
| Ruta + header/cards **atómicos** | `course-details-dynamic`, `blog-details-dynamic` | `Missing required parameter` si se cambian por separado |
| `{slug}` sin route-model-binding (slugs colisionan) | `lesson-player-from-db` | Resolución manual con `firstWhere('slug')` scoped al curso |
| `->through()` ≠ `->map()->all()` sobre paginador | `blog-details-dynamic` | El paginador no tiene `map()` en el retorno esperado |
| `->skip(N)->paginate(M)` offset suma | `blog-details-dynamic` | Page 2 → offset = N+M (consistente, no un bug) |
| `assertSee('>X<')` escapa HTML | `blog-details-dynamic` | Usar `escape: false` para conteos/herramientas HTML |
| `published_at ?? created_at` fallback | `blog-details-dynamic`, `course-details-dynamic` | Posts/cursos sin fecha publicación explícita |
| `config:clear` + `route:clear` antes de tests | Todos los specs | `Route [X] not defined` si el cache está stale |
| PHP portable DB-agnostic (sin `whereRaw`) | `admin-dashboard-lms`, `lesson-player-from-db` | Tests SQLite vs MySQL |
| Helpers en `tests/Pest.php` (createTestX) | `blog-details-dynamic`, `course-details-dynamic`, `instructor-profile` | Tests con datos específicos sin Faker |
| Scope `published()` en queries públicas | Todos los specs de vistas públicas | Solo contenido publicado se muestra |
| Datos determinísticos sin Faker | Todos los specs | Tests reproducibles, sin dependencias externas |
| Null-ocultado con `@if` (nivel 5) | Todos los specs de vistas públicas | Campos null → contenedor oculto, nunca placeholder |
| Commit spec rojo primero, feat verde al final | Todos los specs | Flujo Speckit: spec → tests rojos → implementación → docs → commit |

---

## Checklist de verificación final

- [ ] Tests de la feature en verde (N tests)
- [ ] Contrato `PublicRoutesTest` actualizado si la ruta cambió
- [ ] Suite completa en verde (`php artisan test`)
- [ ] `migrate:fresh --seed` ejecutado en MySQL local sin errores
- [ ] `AGENTS.md` actualizado (feature block, fila cobertura, conteos, hardcodes restantes)
- [ ] `docs/modelo-de-datos.md` actualizado (tabla, matriz, roadmap, fecha)
- [ ] `README.md` actualizado (tabla vistas, rutas, hardcodes)
- [ ] `docs/rutas-y-accesos.md` actualizado (grupo A)
- [ ] Commit spec en rojo
- [ ] Commit feat en verde
- [ ] Rama `dev` y `main` sincronizadas (si aplica)
