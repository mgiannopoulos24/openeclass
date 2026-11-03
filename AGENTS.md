# AGENTS.md — Open eClass

Open eClass is a PHP (>= 8.2) course management system by GUnet, backed by
MySQL/MariaDB. It is a classic page-per-script app: every `.php` under
`modules/`, `main/`, `admin/` etc. is a URL. There is no router, no ORM, and
global state is used heavily. Production installs run at many universities, so
**security and backwards compatibility matter more than elegance**.

## Layout

| Path | What lives there |
|---|---|
| `include/init.php` | Bootstrap: config, DB, session, CSRF token, language, role flags, `require_*` checks. |
| `include/baseTheme.php` | Includes `init.php`, **enforces** the access checks, defines `view()` / `draw()`. |
| `include/main_lib.php` | Global helpers (`q()`, `purify()`, `standard_text_escape()`, CSRF, tokens, `redirect_to_home_page()`, `get_config()` …). |
| `include/lib/` | Shared libraries: uploads (`fileUploadLib.inc.php`), downloads (`forcedownload.php`), `session.class.php`, `hierarchy.class.php`, … |
| `include/constants.php` | `ECLASS_VERSION`, `MODULE_ID_*`, `USER_*` status constants. |
| `modules/<tool>/` | One directory per course tool / feature. |
| `modules/db/database.php` | `Database` class (PDO wrapper with typed placeholders). `dbhelper.php` has `DBHelper`. |
| `resources/views/` | Blade templates (`jenssegers/blade`), rendered with `view('modules.x.y', $data)`. |
| `lang/{el,en,de,es,fr,it}/` | Message files: `$langFoo = "...";` globals. |
| `install/install_db.php` | Full schema for fresh installs. |
| `upgrade/functions.php`, `upgrade/upgrade.php` | Versioned migrations (`upgrade_to_4_5()` etc.). |
| `api/v1/` | Token-authenticated JSON API (`Access` class). |
| `courses/`, `video/`, `config/` | Runtime data (uploads, config.php). Never commit content from these. |
| `vendor/`, `node_modules/`, `js/` | Third-party code. Don't edit vendored libraries in place. |

## Run / develop

- Docker: `docker compose -f docker-compose.development.yaml up` (MariaDB 10.11, app on :80, source bind-mounted).
- PHP deps: `composer install`. JS deps: `npm install` (postinstall runs `php ./js/build/build.php`).
- There is no test suite or linter on `master`. At minimum run `php -l <file>` on every PHP file you touch
  and exercise the page in a browser as each affected role (student, teacher/editor, admin, guest).

## Non-negotiable rules

1. **Pages include `include/baseTheme.php`, not `include/init.php`.** `init.php` only *sets*
   `$toolContent_ErrorExists` when a `require_*` check fails; `baseTheme.php` is what redirects and
   `exit`s. Including `init.php` directly silently disables access control (see upstream fix f9c4274d0).
2. **Declare access requirements before the include**: `$require_current_course`, `$require_login`,
   `$require_editor`, `$require_course_admin`, `$require_admin`, `$require_power_user`, `$guest_allowed`, …
   Then still check `$is_editor` / `$is_course_admin` around every privileged branch.
3. **All SQL goes through `Database::get()` with typed placeholders** (`?d` int, `?s` string, `?f` float,
   `?b` bool, `?t` datetime). Never interpolate request data into SQL. Column/table names and `ORDER BY`
   fields come from a whitelist.
4. **Scope every query by owner**: course data `AND course_id = ?d` with the session's `$course_id`,
   user data `AND user_id = ?d` with `$uid`. IDs from the request are never trusted alone (IDOR).
5. **CSRF on every state change**: forms emit `{!! generate_csrf_token_form_field() !!}`; handlers start with
   `if (!isset($_POST['token']) || !validate_csrf_token($_POST['token'])) csrf_token_error();`.
   State changes happen on POST; GET actions that mutate must carry and check the token too.
6. **Escape output**: Blade `{{ }}` by default; plain PHP uses `q()`; JS strings use `js_escape()` or
   `json_encode()`. Rich user HTML goes through `purify()` / `standard_text_escape()` before `{!! !!}`.
7. **Uploads** go through `validateUploadedFile()` (extension whitelist + PHP-extension blacklist), are
   stored under `courses/` with `safe_filename()` names, and are served with `send_file_to_client()`.
8. **Schema changes** go in both `install/install_db.php` and the current `upgrade_to_X_Y()` in
   `upgrade/functions.php`, guarded with `DBHelper::tableExists/fieldExists/indexExists`.
9. **New UI strings** go into all six `lang/*/messages.inc.php` files (Greek in `el`, English in the rest
   until translated). Never hard-code user-visible text.
10. Do not weaken or bypass existing security helpers, and never commit secrets, `config/config.php`,
    or course data.

Detailed checklists live in skills (same content in `.agents/skills/` and `.claude/skills/`):

- `openeclass-security` — security rules and review checklist for any change.
- `openeclass-module-page` — anatomy of a module page, Blade views, flash messages, logging, language strings.
- `openeclass-db-schema` — `Database` API details and how to add or alter tables safely.

## Style

- Match the surrounding file: 4-space indents, `and`/`or` are common in older code, `snake_case`
  functions, global `$langX` variables pulled in with `global` inside functions.
- Keep the GPL file header on new PHP files (copy from a neighbour).
- Prefer small, targeted diffs. Don't reformat or modernise code you aren't changing.
- Commit subjects follow upstream: `Module: short description` (e.g. `Glossary: fix category ordering`).

## Security reports

Vulnerabilities are reported privately (see `SECURITY.md`). Do not put proof-of-concept exploits for
unfixed issues in commits, PRs, or public forks.
