---
name: openeclass-security
description: Security rules and review checklist for Open eClass PHP code. Use when writing or reviewing any page, AJAX endpoint, API call, form handler, upload/download, or query in this repo, and when auditing for access control, IDOR, CSRF, SQL injection, XSS, path traversal, or open redirects.
---

# Open eClass security

Open eClass has no framework-level protection: each script is responsible for its own access
checks, CSRF validation, query parameterisation, and output escaping. Most real vulnerabilities
fixed upstream are **missing checks**, not broken helpers. Apply every section below that touches
your change.

## 1. Bootstrap and access control

Set requirement flags, then include the theme:

```php
$require_current_course = true;   // needs ?course=CODE, populates $course_id, $course_code
$require_editor = true;           // or $require_course_admin / $require_admin / $require_power_user / $require_login
// $guest_allowed = true;         // only if guests may see this page
require_once '../../include/baseTheme.php';
```

- **Always include `include/baseTheme.php`.** `include/init.php` only *records* a failed check in
  `$toolContent_ErrorExists`; `baseTheme.php` acts on it (redirect + `exit`). A page that includes
  `init.php` directly runs with no enforcement. Existing direct includes (`api/index.php`, `rss.php`,
  `modules/document/file.php`, `modules/mobile/minit.php`, …) do their own checks; if you touch one,
  verify the checks are still complete.
- Role variables set by init: `$uid`, `$is_admin`, `$is_power_user`, `$is_usermanage_user`,
  `$is_departmentmanage_user`, `$is_editor`, `$is_course_admin`, `$is_course_reviewer`,
  `$is_consultant`, `$is_coordinator`, `$session->status`. Student view (`$_SESSION['student_view']`)
  clears the editor flags, so don't re-derive privileges from the session.
- Pages mixing student and teacher actions must wrap each privileged branch in `if ($is_editor) { … }`
  (or the stricter flag), including the **POST handler**, not just the form that renders it.
- Course tools must respect module visibility: init blocks disabled modules via `current_module_id()`
  for non-editors. For resources inside a module, also check the item's own `visible` flag and any
  group/unit/session restriction (`resource_access()`, `check_user_belongs_in_session()`,
  `user_is_registered_to_course()`).
- AJAX handlers and `*_ajax.php` endpoints need the same flags and checks as the page that calls them.

## 2. IDOR: scope every lookup

Request IDs are attacker-controlled. Every SELECT/UPDATE/DELETE on course data includes the
session's course, and per-user data includes `$uid`:

```php
$item = Database::get()->querySingle("SELECT * FROM announcement WHERE id = ?d AND course_id = ?d",
                                     $_GET['id'], $course_id);
if (!$item) { forbidden(); }       // or redirect with a flash message
Database::get()->query("DELETE FROM announcement WHERE id = ?d AND course_id = ?d", $id, $course_id);
```

- Child tables without `course_id` must join to their parent to prove ownership
  (e.g. `exercise_question` → `exercise.course_id`).
- Bulk actions (`$_POST['selectedcbids']`) apply the scope to *each* ID.
- Never trust a hidden `course_id`, `user_id`, or `uid` form field: use `$course_id` / `$uid`.
- `getIndirectReference()` / `getDirectReference()` map IDs to per-session random tokens in some
  forms. It hides IDs but is **not** an authorisation check: still scope the query.

## 3. CSRF

- Forms: `{!! generate_csrf_token_form_field() !!}` (Blade) or `generate_csrf_token_form_field()` in PHP.
- Links/AJAX that change state: append `generate_csrf_token_link_parameter()` (`token=…`) or send
  `token` in the POST body.
- Handler, first line inside the branch:
  ```php
  if (!isset($_POST['token']) || !validate_csrf_token($_POST['token'])) csrf_token_error();
  ```
- Do state changes on POST. If an existing GET action mutates data (delete links, toggles), validate
  `$_GET['token']` the same way.
- `token_generate($info, true)` / `token_validate($info, $token, $seconds)` create HMAC tokens keyed by
  `code_key` for signed links (password reset, iCal, unsubscribe). Bind `$info` to the user and
  resource, and set an expiry.

## 4. SQL

- Use `Database::get()->query/querySingle/queryArray/queryFunc` with typed placeholders
  `?d ?s ?f ?b ?t`. Arrays passed as arguments are flattened, so `IN (" . implode(',', array_fill(0, count($ids), '?d')) . ")"` works with `$ids`.
- The wrapper splits on every `?`. Don't put a literal `?` in SQL text (e.g. in a string literal or
  `REGEXP`). Pass it as a `?s` parameter instead.
- Identifiers (`ORDER BY` column, direction, table name) can't be parameters: map request values
  through a whitelist array, never concatenate them.
- `LIKE` patterns: pass `'%' . $term . '%'` as `?s`. Escape `%`/`_` if the user shouldn't control
  wildcards.
- Casting with `intval()` is fine for building constant IDs, but placeholders are still preferred.

## 5. Output encoding (XSS)

| Context | Use |
|---|---|
| Blade text / attribute | `{{ $value }}` (escapes with `ENT_QUOTES`) |
| PHP-built HTML | `q($value)`; `q_math()` where `[m]` math is allowed |
| Inline JS string | `js_escape($value)` inside quotes, or `json_encode($value)` |
| URL parameter | `urlencode()` then `q()` for the attribute |
| User rich text (TinyMCE) | store `purify($_POST['x'])`; render `{!! standard_text_escape($x) !!}` |
| JSON responses | `header('Content-Type: application/json'); header('X-Content-Type-Options: nosniff');` + `json_encode` |

- `{!! !!}` is only for trusted HTML: helper-built markup (`action_bar()`,
  `generate_csrf_token_form_field()`, `$head_content`), purified HTML, or language strings.
  Never for raw DB or request values. When building HTML in PHP for a `{!! !!}` slot, `q()` every
  dynamic piece yourself.
- `$langX` strings containing `%s` are filled with `sprintf()`; escape the arguments, not the template.
- Titles, filenames, usernames, and comments are user input too. Escape them in alerts, breadcrumbs
  (`$navigation`), `$toolName`, emails, and `Log::record()` displays.

## 6. Uploads

```php
require_once 'include/lib/fileUploadLib.inc.php';
validateUploadedFile($_FILES['userFile']['name'], 2);           // exits on disallowed extension; 'json' for AJAX
$stored = safe_filename(get_file_extension($_FILES['userFile']['name']));
move_uploaded_file($_FILES['userFile']['tmp_name'], "$webDir/courses/$course_code/<area>/$stored");
```

- Always validate against the whitelist (`student_upload_whitelist` / `teacher_upload_whitelist`).
  `isWhitelistAllowed()` also hard-blocks `.php*`, `.phtml`, `.phar`. Never add bypasses.
- Store under a server-generated name (`safe_filename()`); keep the original name only in the DB.
- Check quota (`enough_size()` / `get_max_upload_size()`).
- Archives: follow `modules/document/index.php`. Run `validateUploadedFile(my_basename($name))` on
  every entry first, then rename each one via `process_extracted_file()` and extract only that entry.
  Never `ZipArchive::extractTo($dir)` a whole archive (zip slip plus executable content).
- Validate filenames on rename with `validateRenamedFile()`.

## 7. Downloads and file paths

- Serve files with `send_file_to_client($real_path, $display_name, …)`, which rejects control chars
  and backslashes and sets MIME/charset. Don't `readfile()` a user-influenced path.
- Resolve paths from DB rows (e.g. `public_path_to_disk_path()` for documents), not by concatenating
  request input. If you must build a path, use `basename()`/`my_basename()`, reject `..`, and verify
  `realpath()` stays under the expected base directory.
- Check the resource's visibility and the user's access before sending the file.

## 8. Redirects, URLs, outbound requests

- Internal redirects: `redirect_to_home_page('modules/x/index.php?course=' . $course_code)` (relative to
  `$urlServer`). For `next=`/`return=` parameters, accept only local paths (start with `/`, not `//`,
  no scheme), not arbitrary URLs.
- Server-side HTTP fetches (`CurlUtil`, Guzzle) to user-supplied URLs are SSRF risks: restrict scheme
  to http/https and avoid internal hosts, or limit them to admin-configured endpoints.

## 9. Sessions, auth, secrets

- Passwords: `password_hash(..., PASSWORD_DEFAULT)` / `password_verify()`. Never log or email them.
- Random values: `generate_csrf_token()` / `random_bytes()`, never `rand()`/`mt_rand()`/`uniqid()`
  for secrets.
- Compare secrets with `hash_equals()` in new code.
- `unserialize()` only on data the server wrote itself (DB/config). For anything from the
  client use `json_decode()`, or `unserialize($x, ['allowed_classes' => false])`.
- No `eval`, `create_function`, backticks, or `shell_exec`/`exec` with request data. If a shell call is
  unavoidable, wrap each argument in `escapeshellarg()`.
- Don't print stack traces or SQL errors to users. `Database` reports via the `Debug` class.

## 10. API (`api/v1/`)

- Each endpoint goes through `api/v1/apiCall.php` → `Access::fromToken()`. Check
  `$access->isValid` and the token's permissions before acting. Return errors with `Access::error()`.
- Apply the same course/user scoping as the web UI. A valid token is not a licence to read any course.

## Review checklist

- [ ] Includes `baseTheme.php`, sets the right `$require_*` / `$guest_allowed` flags.
- [ ] Every privileged branch, including POST and AJAX handlers, is guarded by the right role flag.
- [ ] Every query uses placeholders; identifiers come from whitelists; no literal `?` in SQL text.
- [ ] Every query on an ID from the request is scoped by `course_id = ?d` / `user_id = ?d` or joined to its owner.
- [ ] Every state change validates the CSRF token.
- [ ] All output is escaped for its context; `{!! !!}` only on trusted or purified HTML.
- [ ] Uploads are whitelisted, renamed, and quota-checked; downloads are access-checked and path-safe.
- [ ] Redirect targets are internal; outbound URLs are restricted.
- [ ] Hidden resources, disabled modules, and student-view mode behave correctly.
- [ ] Sensitive actions are logged with `Log::record()` without secrets.
