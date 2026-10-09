# Running Open eClass locally

This guide gets a fully installed Open eClass running in Docker, with no
installation wizard. When you finish, open the site and log in.

## Requirements

- Docker with the Compose plugin (`docker compose version`)
- Port 80 free on your machine (see [Using another port](#using-another-port))

## Quick start

Run all commands from the repository root.

```sh
# 1. Build the image (about 1–2 minutes the first time)
docker compose -f docker-compose.build.yaml build

# 2. Start eClass and MariaDB
docker compose up -d

# 3. Install non-interactively
docker compose exec -u nginx \
    -e BASE_URL=http://localhost/ \
    -e ADMIN_PASSWORD=secret \
    eclass php install/index.php
```

Step 3 ends with:

```
Success: Open eClass installation complete
Base URL: http://localhost/
Admin username: admin
Admin password: secret
```

Open <http://localhost/> and log in with those credentials.

> You must build the image (step 1) yourself.
> `docker-compose.yaml` refers to `ghcr.io/gunet/openeclass`, which is not
> published, so `docker compose up` cannot pull it.

## Notes

- **Run the installer as the `nginx` user** (`-u nginx`). The web server runs
  as `nginx`, so `config/config.php` and the `courses/` files must belong to it.
- **`BASE_URL` must match the address you use in the browser**, including the
  port and the trailing slash. Links and redirects are built from it.
- **The admin password:** set it with `ADMIN_PASSWORD`. If you omit it, the
  installer generates a random password and prints it at the end.
  The username comes from `ADMIN_USERNAME` (`admin` by default) in
  `docker-compose.yaml`.
- **Data persists** in Docker volumes (database, `config/`, `courses/`,
  `video/`), so `docker compose down` / `up -d` keeps your installation.
  To reinstall, see below.

## Common tasks

| Task | Command |
|---|---|
| Stop | `docker compose down` |
| Start again | `docker compose up -d` |
| Logs | `docker compose logs -f eclass` |
| Shell in the container | `docker compose exec -u nginx eclass sh` |
| Reset everything (deletes all data) | `docker compose down -v` |

After a reset, repeat steps 2 and 3. Run step 3 only once per installation.
On an installation that already exists it fails with a PHP database error
(`SQLSTATE[HY000] [2002]`), but leaves the installation unchanged.

To pick up code changes, rebuild the image (step 1) and run
`docker compose up -d`. Your data volumes are kept.

## Using another port

If port 80 is taken, change the mapping in `docker-compose.yaml`
(for example `"8080:80"`) and pass the same port in `BASE_URL`:

```sh
docker compose exec -u nginx \
    -e BASE_URL=http://localhost:8080/ \
    -e ADMIN_PASSWORD=secret \
    eclass php install/index.php
```

## Testing SSO and LDAP

To add the test CAS server and LDAP directory, start the stack with the extra
compose file, then install as in step 3:

```sh
docker compose -f docker-compose.yaml -f docker-compose.test.yaml up -d
```

See [`docker/README.md`](../docker/README.md) for the image's environment
variables.
