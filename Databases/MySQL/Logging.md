## 🔹 1. Standalone MySQL Installation

**Config file location (my.cnf / my.ini):**

* **Linux:**

  ```
  /etc/mysql/my.cnf
  /etc/my.cnf
  /etc/mysql/mysql.conf.d/mysqld.cnf
  ```
* **Windows:**

  ```
  C:\ProgramData\MySQL\MySQL Server <version>\my.ini
  C:\Program Files\MySQL\MySQL Server <version>\my.ini
  ```
* **macOS (Homebrew):**

  ```
  /usr/local/etc/my.cnf
  ```

**Data directory (default):**

* Linux: `/var/lib/mysql/`
* Windows: `C:\ProgramData\MySQL\MySQL Server <version>\Data\`
* macOS: `/usr/local/var/mysql/`

---

## 🔹 2. Bundled MySQL Installation

Some applications ship their own MySQL server inside their directory.

* Each bundled MySQL has its own `my.cnf` (or `my.ini`) and data directory.
* It is completely separate from the system-wide MySQL installation.

**Example (Windows):**

```
C:\Program Files (x86)\SomeApp\mysql\my.ini
C:\Program Files (x86)\SomeApp\mysql\data\
```

**Example (Linux):**

```
/opt/someapp/mysql/my.cnf
/opt/someapp/mysql/data/
```

---

## 🔹 3. Logging Configuration (Global)

Edit `my.cnf` (or `my.ini`) under `[mysqld]`:

```ini
[mysqld]
general_log = 1
general_log_file = /var/log/mysql/mysql.log
log_output = FILE
```

**Other useful logs:**

* **Error log:**

  ```
  log_error = /var/log/mysql/error.log
  ```
* **Slow query log:**

  ```ini
  slow_query_log = 1
  slow_query_log_file = /var/log/mysql/mysql-slow.log
  long_query_time = 1
  ```

**Reload configuration:**

* Linux:

  ```bash
  sudo systemctl restart mysql
  ```
* Windows: restart "MySQL" service from Services panel.

Or dynamically (without restart) inside MySQL:

```sql
SET GLOBAL general_log = 'ON';
SET GLOBAL slow_query_log = 'ON';
```

---

## 🔹 4. Per-Database / Per-User Logging

Unlike PostgreSQL, MySQL **does not support per-database or per-user logging directly** in config.
Instead you can:

* Enable **general log** and then filter queries by `db` or `user` column inside the log table (`mysql.general_log`) if using `log_output = TABLE`.

  ```sql
  SET GLOBAL log_output = 'TABLE';
  SELECT * FROM mysql.general_log WHERE user_host LIKE 'appuser%';
  ```
* Or use **per-session logging**:

  ```sql
  SET SESSION sql_log_off = 0;
  ```

---

## 🔹 5. Best Practices

* Do **not** keep `general_log` enabled in production → it logs *every* query and creates massive logs.
* Use **slow query log** to capture only problematic queries.
* Ensure log files are rotated regularly (`logrotate` on Linux, Windows Task Scheduler).

---

✅ **Key Standard:**

* Identify if you are editing **Standalone MySQL** or **Bundled MySQL**.
* Global logging is controlled in `my.cnf` / `my.ini` under `[mysqld]`.
* For project-level isolation, use **log\_output = TABLE** and filter by database/user.
* Use **general log for debugging** and **slow query log for production tuning**.
