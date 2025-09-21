## 🔹 1. Standalone Oracle Database Installation

**Configuration and data files:**

* **Config file (`init.ora` / `spfile.ora`):**

  * Linux:

    ```
    $ORACLE_HOME/dbs/init<SID>.ora
    $ORACLE_HOME/dbs/spfile<SID>.ora
    ```
  * Windows:

    ```
    C:\oracle\product\<version>\dbhome_1\dbs\init<SID>.ora
    C:\oracle\product\<version>\dbhome_1\dbs\spfile<SID>.ora
    ```
  * `$ORACLE_HOME` is the root of the Oracle installation.
  * `<SID>` is the database instance identifier.

**Data files (default):**

* Linux: `$ORACLE_BASE/oradata/<DB_NAME>/`
* Windows: `C:\oracle\oradata\<DB_NAME>\`

---

## 🔹 2. Bundled Oracle Database

Some applications ship with **embedded or private Oracle instances** (often Express Edition – XE).

* Bundled Oracle has its **own ORACLE\_HOME**, **SID**, and **data directory**.
* Completely isolated from any system-wide Oracle installation.

**Example (Windows, AppX):**

```
C:\Program Files (x86)\SomeApp\oracle\oradata\<DB_NAME>\
C:\Program Files (x86)\SomeApp\oracle\product\XE\dbhome_1\spfileXE.ora
```

**Example (Linux, AppY):**

```
/opt/someapp/oracle/oradata/<DB_NAME>/
```

---

## 🔹 3. Logging Configuration (Global)

### 3.1 Alert Log

* Captures startup, shutdown, and critical errors.
* **Default locations:**

  * Linux: `$ORACLE_BASE/diag/rdbms/<DB_NAME>/<SID>/trace/alert_<SID>.log`
  * Windows: `C:\oracle\diag\rdbms\<DB_NAME>\<SID>\trace\alert_<SID>.log`

### 3.2 Listener Log

* Logs connection requests:

  * Linux: `$ORACLE_BASE/diag/tnslsnr/<HOST>/<LISTENER>/trace/listener.log`
  * Windows: `C:\oracle\diag\tnslsnr\<HOST>\<LISTENER>\trace\listener.log`

### 3.3 SQL Query / Audit Logging

* Oracle does **not have a direct `log_statement = 'all'`**. Options:

  * **AUDIT commands** (per user or object):

    ```sql
    AUDIT SELECT TABLE, INSERT, UPDATE, DELETE BY <username> BY ACCESS;
    ```
  * **Unified Audit (12c+)** – capture all statements by users or roles:

    ```sql
    CREATE AUDIT POLICY all_statements
    ACTIONS ALL ON DATABASE;
    AUDIT POLICY all_statements;
    ```
  * **Fine-Grained Auditing (FGA)** for specific tables/conditions.

---

## 🔹 4. Per-Database / Per-User Logging

* Oracle logging is **per-user or per-object** rather than per-database.
* You can combine AUDIT policies or FGA policies to capture:

  * Specific users
  * Specific schemas or tables
  * Specific SQL actions

---

## 🔹 5. Best Practices

* Avoid auditing all statements in production → performance impact.
* Use **unified audit / FGA** for selective auditing.
* Rotate logs regularly.
* Bundled Oracle instances are completely isolated → edits to system-wide Oracle don’t affect the bundled one.

---

✅ **Key Standard:**

* Always identify **Standalone Oracle** vs **Bundled Oracle**.
* Global logging (alert, listener) is separate per instance.
* SQL-level auditing is done via **AUDIT / Unified Audit / FGA**.
* Bundled Oracle has its own ORACLE\_HOME, SID, and log directories.
