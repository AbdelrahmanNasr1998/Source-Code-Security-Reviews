## 🔹 1. Standalone SQL Server Installation

**Config file / instance location:**

* SQL Server primarily stores configuration **per instance** inside the registry and system databases (`master`).
* **Default instance (Windows):**

  ```
  C:\Program Files\Microsoft SQL Server\MSSQL<version>.MSSQLSERVER\MSSQL\DATA\
  ```
* **Named instance:**

  ```
  C:\Program Files\Microsoft SQL Server\MSSQL<version>.<InstanceName>\MSSQL\DATA\
  ```
* **Configuration manager** and **SQL Server Management Studio (SSMS)** are used to view/change settings.

---

## 🔹 2. Bundled SQL Server

Some applications ship a **private SQL Server Express instance** inside their directory.

* Each bundled SQL Server has its own **instance name**, **data directory**, and **service**.
* It runs independently from system-wide SQL Server.

**Example (Windows, Application X):**

```
C:\Program Files (x86)\SomeApp\SQLServerExpress\MSSQL\Data\
```

**Example (Linux, via SQL Server on Linux):**

```
/opt/someapp/mssql/data/
```

---

## 🔹 3. Logging / Query Capture Configuration

### 3.1 SQL Server Error Logs

* Location (default):

  ```
  C:\Program Files\Microsoft SQL Server\MSSQL<version>.MSSQLSERVER\MSSQL\Log\ERRORLOG
  ```
* Multiple log files (`ERRORLOG`, `ERRORLOG.1`, …) are rotated automatically.

### 3.2 SQL Server Profiler / Extended Events

* To capture queries (like `log_statement = 'all'` in PostgreSQL):

  * **SQL Server Profiler** (GUI)
  * **Extended Events** (lightweight, preferred for production)

**Example Extended Events session (all queries):**

```sql
CREATE EVENT SESSION [AllQueries] ON SERVER
ADD EVENT sqlserver.sql_statement_completed
ADD TARGET package0.event_file
(SET filename = 'C:\Logs\AllQueries.xel');
ALTER EVENT SESSION [AllQueries] ON SERVER STATE = START;
```

* Captures all completed statements to an `.xel` file.

### 3.3 Trace Flags (Optional)

* Some global flags can increase logging:

  ```sql
  DBCC TRACEON(1222, -1); -- Capture deadlocks
  ```

---

## 🔹 4. Per-Database / Per-User Logging

* SQL Server doesn’t have a direct per-database “log all queries” config.
* Use **Extended Events** or **SQL Audit** to capture:

  * Queries per database
  * Queries per user or role
* Example SQL Audit for a database:

```sql
CREATE SERVER AUDIT [DBAudit]
TO FILE (FILEPATH = 'C:\Logs\Audit\');
ALTER SERVER AUDIT [DBAudit] WITH (STATE = ON);
```

---

## 🔹 5. Best Practices

* Avoid capturing **all queries** in production via Profiler → high overhead.
* Prefer **Extended Events** or **SQL Audit**.
* Rotate and archive logs regularly.
* Use filters (per database, per user, slow queries) to limit log size.

---

✅ **Key Standard:**

* Identify **Standalone SQL Server** vs **Bundled SQL Server**.
* Global logging is configured via **Extended Events / Profiler / SQL Audit**.
* For project-level isolation, create **database-specific sessions or audits**.
* Error logs are always separate per instance.
