# WordPress Docker Template

Run WordPress locally with Docker: **WordPress + MariaDB + phpMyAdmin**. No PHP, MySQL or XAMPP install needed.

- Requirement: install [Docker Desktop](https://www.docker.com/products/docker-desktop/) and **open it** before running any command.
- Run every command in a terminal **inside the project folder** (the folder with `docker-compose.yml`).

---

## 1. First run

```bash
cp .env.example .env
docker compose up -d
```

The first run takes a few minutes to download images. Wait about 30 seconds for the database to start, then open:

- WordPress: <http://localhost:8080>
- phpMyAdmin: <http://localhost:8081> (user `admin`, password `admin`)

Finish the WordPress install wizard in the browser (language, admin account).

All WordPress files (themes, plugins, uploads...) live in the `wordpress/` folder. Edit code there and refresh the browser.

> Default passwords are fine: the site is only reachable from this machine (`localhost`), not from other devices on your network.

---

## 2. Done coding for the day

```bash
docker compose stop
```

Stops the containers. **Database and files are kept.**

---

## 3. Continue coding the next day

Open Docker Desktop, then:

```bash
docker compose up -d
```

The site comes back exactly as you left it.

---

## 4. Start a new project without touching existing ones

Each project is **its own folder** with its own database and files. Docker names everything after the folder, so two folders never share data.

**Step 1:** Clone the template into a new folder with a **unique name**:

```bash
git clone <template-repo-url> new-project-name
cd new-project-name
cp .env.example .env
```

Or copy only these 4 files into a new folder: `docker-compose.yml`, `php-uploads.ini`, `.env.example`, `.gitignore`. **Do not copy** `wordpress/` or `.env` from another project.

**Step 2:** Only if another project is **running at the same time**: open `.env` and pick ports no other project uses:

```env
WP_PORT=8090
PMA_PORT=8091
```

> **Choose the port before the first run.** Changing `WP_PORT` later still works, but images already inserted into posts keep the old port in their URL.

**Step 3:** `docker compose up -d`, then open <http://localhost:8090>.

---

## 5. Import a site with All-in-One WP Migration

1. Install the **All-in-One WP Migration** plugin in WordPress.
2. Copy the `.wpress` file into `wordpress/wp-content/ai1wm-backups/`.
3. In the plugin, open **Backups** and click **Restore** on that file.

> After the restore, log in with the admin account of the **source** site.

---

## 6. Share the site with a public URL (VS Code tunnel, ngrok...)

The site follows whatever URL it is opened from, so tunnels work without changing anything in WordPress.

**VS Code:**

1. Open the **Ports** panel (next to Terminal) and click **Forward a Port**.
2. Enter your `WP_PORT` (e.g. `8080`).
3. Right-click the port, choose **Port Visibility**, then **Public**. Otherwise visitors must sign in to GitHub.
4. Copy the **Forwarded Address** and send it to whoever needs to test.

**ngrok:** `ngrok http 8080`

> Images inserted into post content keep the URL they were uploaded with (e.g. `http://localhost:8080/...`), so they appear broken through the tunnel. Themes, CSS, JS and featured images load fine.
>
> Because the URL is set automatically, the **WordPress Address** and **Site Address** fields in **Settings, General** are greyed out. That is expected.

---

## 7. Remove a project

> ⚠️ **Warning:** These commands permanently delete data. There is no undo.

**Wipe the database, keep the files** (reinstall WordPress on an empty database):

```bash
docker compose down -v
```

**Remove everything:** run the command above, then delete the project folder.

**Keep the database, reinstall WordPress core files:**

```bash
docker compose down
rm -rf ./wordpress
docker compose up -d
```

---

## Useful commands

| Command | What it does |
|---------|--------------|
| `docker compose ps` | Show running containers |
| `docker compose logs -f wordpress` | Follow WordPress logs (Ctrl+C to exit) |
| `docker compose restart` | Restart (e.g. after editing `php-uploads.ini`) |
| `docker compose up -d` | Apply changes after editing `.env` or `docker-compose.yml` |

---

## Configuration (`.env`)

| Variable | Description | Default |
|----------|-------------|---------|
| `WP_PORT` | WordPress port | `8080` |
| `PMA_PORT` | phpMyAdmin port | `8081` |
| `WP_DEBUG` | `1` shows PHP errors and writes `wp-content/debug.log` | `0` |
| `DB_NAME` | Database name | `wordpress` |
| `DB_USER` | Database user (must not be `root`) | `admin` |
| `DB_PASSWORD` | Password for `DB_USER` | `admin` |
| `DB_ROOT_PASSWORD` | MariaDB root password | `admin` |
| `COMPOSE_PROJECT_NAME` | Project name; defaults to the folder name | folder name |

> `DB_*` values only apply on the **first run**, when the database is created. To change them later, wipe the database (`docker compose down -v`) or change them in phpMyAdmin.

PHP settings live in `php-uploads.ini`: 10GB max upload, `memory_limit` 1024M, `max_input_vars` 5000.

---

## Folder structure

```
project-name/
├── docker-compose.yml   ← services
├── php-uploads.ini      ← PHP settings
├── .env.example         ← config template
├── .env                 ← your config (created by you, not committed)
└── wordpress/           ← WordPress files (created on first run)
```
