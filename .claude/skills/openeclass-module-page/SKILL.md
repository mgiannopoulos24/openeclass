---
name: openeclass-module-page
description: How to add or modify a page, form, AJAX endpoint, or Blade view in an Open eClass module (modules/<tool>/), including bootstrap flags, view() rendering, action bars, flash messages, validation, logging, and language strings. Use when building features or fixing bugs in module UI code.
---

# Open eClass module pages

Each script is a URL. A typical course tool page is `modules/<tool>/index.php?course=CODE`. Use an
existing, recently-touched module as a model (`modules/glossary/index.php` with
`resources/views/modules/glossary/*.blade.php` is small and representative). Also follow
`openeclass-security`.

## Page skeleton

```php
<?php
/* GPL header copied from a neighbouring file */

$require_current_course = true;    // course pages; sets $course_id, $course_code, $is_editor …
$require_help = true;
$helpTopic = 'glossary';
// $guest_allowed = true;          // only when guests may view
// $require_editor = true;         // when the whole page is teacher-only

require_once '../../include/baseTheme.php';   // never include/init.php directly
require_once 'include/log.class.php';

$toolName = $langGlossary;
$data['base_url'] = $base_url = 'index.php?course=' . $course_code;

if ($is_editor && isset($_POST['submit'])) {
    if (!isset($_POST['token']) || !validate_csrf_token($_POST['token'])) csrf_token_error();
    $v = new Valitron\Validator($_POST);
    $v->rule('required', ['term']);
    if ($v->validate()) {
        // write with typed placeholders, scoped by $course_id
        Session::flash('message', $langGlossaryAdded);
        Session::flash('alert-class', 'alert-success');
        redirect_to_home_page("modules/glossary/index.php?course=$course_code");
    } else {
        Session::flashPost()->Messages($langFormErrors)->Errors($v->errors());
        redirect_to_home_page("modules/glossary/index.php?course=$course_code&add=1");
    }
}

$data['items'] = Database::get()->queryArray("SELECT ... WHERE course_id = ?d", $course_id);
$data['action_bar'] = action_bar([...]);
view('modules.glossary.index', $data);
```

Key points:

- Bootstrap flags are set **before** `baseTheme.php`. See the security skill for the full list.
- Handle POST first, then **redirect** (PRG pattern) with `redirect_to_home_page()`. Don't render after a
  successful write.
- Feedback: `Session::flash('message', …)` + `Session::flash('alert-class', 'alert-success|alert-warning|alert-danger|alert-info')`.
  Form errors: `Session::flashPost()->Messages(...)->Errors($v->errors())`. In the view, repopulate
  with `Session::get('field')` / `Session::getError('field')`.
- Validation uses Valitron (`Valitron\Validator`), already configured for the UI language in init.
- Breadcrumbs: append `['url' => …, 'name' => …]` to global `$navigation`. Page title via `$toolName` /
  `$pageName`. These are printed in views, so escape user-controlled names with `q()`.
- Assets: `load_js('tools.js')`, `load_js('datatables')`, etc. add to `$head_content`. Inline scripts
  go into `$head_content` too.
- Usage tracking: `require_once 'include/action.php'; (new action())->record(MODULE_ID_X);` on module
  landing pages.
- Audit log after writes: `Log::record($course_id, MODULE_ID_X, LOG_INSERT|LOG_MODIFY|LOG_DELETE, [...details])`.
- `view('modules.<tool>.<file>', $data)` renders `resources/views/modules/<tool>/<file>.blade.php`
  with globals (`$course_code`, `$is_editor`, `$urlAppend`, …) already injected. The legacy path is
  `$tool_content .= …; draw($tool_content, 2, null, $head_content);`. Keep using it in files that
  already do, but put new pages in Blade.

## Blade views

- Extend `layouts.default` and fill `@section('content')`. Copy the wrapper markup
  (`module-container`, left menu, breadcrumbs, `legend_view`, `{!! $action_bar !!}`) from a sibling
  view in the same module.
- `{{ $x }}` for everything by default. `{!! … !!}` only for trusted or purified HTML (see the security skill).
- Language strings in views: `{{ trans('langFoo') }}`, or pass `$langFoo` in via globals.
- Every `<form method="post">` contains `{!! generate_csrf_token_form_field() !!}`.
- UI is Bootstrap 5 + Font Awesome 6 (`fa-solid fa-…`) + jQuery. Reuse existing classes
  (`form-wrapper form-edit`, `submitAdminBtn`, `cancelAdminBtn`, `table-default`) rather than adding CSS.
- Offline course export has its own views under `resources/views/offline/`. If you change data a
  module exposes, check whether the offline view also needs updating.

## AJAX / JSON endpoints

```php
$require_current_course = true;
require_once '../../include/baseTheme.php';
if (!$is_editor) { forbidden(); }
if (!isset($_POST['token']) || !validate_csrf_token($_POST['token'])) csrf_token_error();
header('Content-Type: application/json; charset=utf-8');
header('X-Content-Type-Options: nosniff');
echo json_encode($result);
exit;
```

From JS, send the token you rendered into the page (e.g. `data-token="{{ $_SESSION['csrf_token'] }}"`
or a hidden field) and include `course=` in the URL so the course context loads.

## Language strings

- Messages are PHP globals in `lang/<code>/messages.inc.php` (`common.inc.php` for shared terms,
  `messages_collaboration.inc.php` for collaboration mode). Languages: `el, en, de, es, fr, it`.
- Adding a string: add `$langMyNewThing = "...";` to **all six** `messages.inc.php` files, near related
  keys. Greek text goes in `el`, English in `en`, and the English text as a placeholder in `de/es/fr/it`
  (upstream's practice until translations arrive).
- Use the variable as a global (`global $langX;` inside functions) or `trans('langX')`. Never
  hard-code UI text in PHP or Blade.
- Use `sprintf($langX, q($arg))` for parameterised strings.
- Sites override messages in `config/<lang>.inc.php`. Don't rename existing keys without a reason;
  that breaks local overrides.

## New course tool checklist

1. `MODULE_ID_*` constant in `include/constants.php`.
2. Entries in the module arrays in `include/init.php` (title/link/icon; there are several parallel arrays,
   grep for an existing `MODULE_ID_GLOSSARY` and mirror every occurrence).
3. Insert the `course_module` row for new and existing courses (install + upgrade; see `openeclass-db-schema`).
4. Language strings, help topic, Blade views.
5. Respect module visibility and `$is_editor` in every script of the module.

## Verify

- `php -l` every changed PHP file.
- Load the page as teacher, student, a non-registered user, and a guest. Toggle the module and the
  item's visibility. Try student view (`$_SESSION['student_view']`).
- Submit forms with a missing or wrong token: they must be rejected.
