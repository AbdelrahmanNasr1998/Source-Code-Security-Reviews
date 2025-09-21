## 🔹 1. Standalone PostgreSQL Installation

**Config file location (`postgresql.conf`):**

* **Linux:**

  ```
  /etc/postgresql/<version>/main/postgresql.conf
  /var/lib/postgresql/data/postgresql.conf
  ```
* **Windows:**

  ```
  C:\Program Files\PostgreSQL\<version>\data\postgresql.conf
  ```
* **macOS (Homebrew):**

  ```
  /usr/local/var/postgres/postgresql.conf
  ```

**Log directory (default):**

* Linux: `/var/log/postgresql/` or `<data_dir>/pg_log/`
* Windows: `C:\Program Files\PostgreSQL\<version>\data\pg_log\`
* macOS: `/usr/local/var/postgres/pg_log/`

---

## 🔹 2. Bundled PostgreSQL Installation

Some applications ship with their **own PostgreSQL cluster** bundled inside their installation directory.

* Each bundled DB has its own **data directory** and **postgresql.conf** file.
* It is isolated from any standalone PostgreSQL instance.

**Example (Windows):**

```
C:\Program Files (x86)\SomeApp\pgsql\data\postgresql.conf
C:\Program Files (x86)\SomeApp\pgsql\data\pg_log\
```

**Example (Linux):**

```
/opt/someapp/pgsql/data/postgresql.conf
/opt/someapp/pgsql/data/pg_log/
```

---

## 🔹 3. Logging Configuration (Global)

In `postgresql.conf`:

```conf
log_statement = 'all'           # options: none | ddl | mod | all
logging_collector = on
log_directory = 'pg_log'
log_filename = 'postgresql-%Y-%m-%d.log'
```

**Reload configuration:**

* Linux:

  ```bash
  sudo systemctl reload postgresql
  ```
* Windows: restart PostgreSQL service from Services panel.
* SQL method (works in both):

  ```sql
  SELECT pg_reload_conf();
  ```

---

## 🔹 4. Logging Configuration (Overrides)

Instead of changing the global config, you can override logging for specific scopes:

* **Per Database:**

  ```sql
  ALTER DATABASE mydb SET log_statement = 'all';
  ```
* **Per User (Role):**

  ```sql
  ALTER ROLE myuser SET log_statement = 'all';
  ```

---

## 🔹 5. Best Practices

* Use `log_statement = 'all'` only for debugging/auditing (it generates very large logs).
* For production:

  * Use `log_statement = 'mod'` or `ddl`.
  * Or set `log_min_duration_statement = 1000` to log only slow queries (>1 second).

---

✅ **Key Standard:**

* Always identify whether you are editing **Standalone PostgreSQL** or a **Bundled PostgreSQL**.
* Edit the correct `postgresql.conf` under the proper data directory.
* Use **global config** for all DBs, or `ALTER DATABASE` / `ALTER ROLE` for project-level logging.
