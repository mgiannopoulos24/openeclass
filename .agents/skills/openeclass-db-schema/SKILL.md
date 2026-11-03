---
name: openeclass-db-schema
description: Using the Open eClass Database class (typed ?d/?s placeholders, querySingle/queryArray/queryFunc, transactions) and making schema changes safely across install/install_db.php and upgrade/functions.php. Use when writing SQL, adding or altering tables, columns, indexes, config keys, or course modules.
---

# Open eClass database and schema changes

## Database API (`modules/db/database.php`)

```php
$db = Database::get();                         // the platform DB ($mysqlMainDb from config/config.php)
$row  = $db->querySingle("SELECT * FROM course WHERE id = ?d", $id);        // object|null/false
$rows = $db->queryArray("SELECT * FROM glossary WHERE course_id = ?d", $course_id);  // array of objects
$db->queryFunc("SELECT id, name FROM glossary_category WHERE course_id = ?d",
    function ($r) use (&$cats) { $cats[$r->id] = $r->name; }, $course_id);   // return true to stop early
$res  = $db->query("INSERT INTO glossary (term, course_id) VALUES (?s, ?d)", $term, $course_id);
$newId = $res->lastInsertID;  $n = $res->affectedRows;
```

Placeholders (the wrapper rewrites them into PDO bound parameters):

| Token | Type | Notes |
|---|---|---|
| `?d` | int | value passed through `intval()` |
| `?s` | string | |
| `?f` | float | |
| `?b` | bool | |
| `?t` | datetime string | `'Y-m-d H:i:s'` |
| `?` | auto-guess | logs a warning; don't use in new code |

- `null` arguments bind as SQL NULL whatever the type letter.
- Array arguments are flattened in order, so one array can feed many placeholders (e.g. multi-row
  `VALUES (?d, ?d, ?d), …` or `IN (...)` lists).
- The statement is split on **every** `?`, so a literal `?` anywhere in the SQL text breaks the query.
  Pass such values as parameters.
- Too few arguments is fatal (`die()`); too many is logged. Count your placeholders.
- Errors are reported through `Debug` and return null/false; check results before using them.
- Never concatenate request data into SQL. Identifiers (ORDER BY column, direction) come from a whitelist map.
- Transactions:
  ```php
  $ok = Database::get()->transaction(function () use (...) {
      // queries…
      return Database::TRANSACTION_SUCCESS;     // anything else rolls back
  });
  ```
- Charset/engine for new tables: use the `$tbl_options` passed into install/upgrade functions
  (`DEFAULT CHARACTER SET=utf8mb4 COLLATE utf8mb4_bin ENGINE=InnoDB`). Don't hard-code it.
- Config values live in the `config` table: read with `get_config('key', $default)`, write with
  `set_config('key', $value)`. New keys need a default when read. Seed them in install, and in upgrade
  if the default isn't enough.

## Schema change procedure

A schema change has to work for **fresh installs and upgrades**:

1. **`install/install_db.php`**: add the final shape of the table/column/index to the CREATE TABLE
   statements (fresh installs never run upgrade functions).
2. **`upgrade/functions.php`**: add idempotent steps to the current version's function
   (`upgrade_to_4_5($tbl_options)` while `ECLASS_VERSION` in `include/constants.php` is `4.5-dev`):
   ```php
   if (!DBHelper::tableExists('my_table')) {
       Database::get()->query("CREATE TABLE `my_table` (
           `id` INT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
           `course_id` INT(11) NOT NULL,
           ...,
           KEY `course_id` (`course_id`),
           FOREIGN KEY (`course_id`) REFERENCES `course` (`id`) ON DELETE CASCADE
       ) $tbl_options");
   }
   if (!DBHelper::fieldExists('my_table', 'new_col')) {
       Database::get()->query("ALTER TABLE `my_table` ADD `new_col` VARCHAR(255) DEFAULT NULL AFTER `x`");
   }
   if (!DBHelper::indexExists('my_table', 'my_idx')) { ... }
   ```
   Every step must be safe to run twice, since upgrades can be re-run after a failure.
3. Only when starting a new release line: add a new `upgrade_to_X_Y()`, its entry in the `$versions`
   list, and the matching `elseif ($version === 'X.Y')` branch in `upgrade/upgrade.php`. Long upgrades
   can be split into steps with `break_on_step()` / `steps_finished()` (see the 4.4 branch).
4. **New course modules**: `finalize_upgrade()` re-inserts every `$modules` entry into `course_module`
   for all courses (`INSERT IGNORE`), so registering the module in `include/init.php` / `constants.php`
   covers existing courses, and course creation (`modules/create_course/functions.php`) seeds from
   `$modules` too. If it should be on by default in new courses, add it to `default_modules()`.
5. Deleting data: course-scoped tables should `REFERENCES course(id) ON DELETE CASCADE`, or be cleaned
   up in the course deletion code (`delete_course()` and friends). Otherwise deleted courses leave orphans.
6. Keep MySQL **and** MariaDB compatibility (dev Docker uses MariaDB 10.11): avoid vendor-only syntax
   and check `JSON`/`CHECK`/generated-column support before using it.

## Gotchas

- Large tables (`actions_daily`, `log`, `logins`, `exercise_answer_record`, `document`): adding indexes
  or columns can take minutes on big installs. Mention it in the commit and avoid needless rewrites.
- Don't rename or drop columns that other modules or `api/v1` read without updating every reader:
  grep the column name across `modules/`, `include/`, `api/`, `main/`, `resources/views/`.
- Always use `Database::get()`. `Database::core()` is a server-level connection with no database
  selected (used by the installer for `CREATE DATABASE`), not "the main DB".

## Verify

- `php -l install/install_db.php upgrade/functions.php`.
- Fresh install in Docker (`docker compose -f docker-compose.development.yaml up`) succeeds.
- Upgrade path: restore a DB from the previous release (or set `config.version` back), run
  `upgrade/` and confirm the step runs once and is a no-op on a second run.
