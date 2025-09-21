## 🔹 1. Database Files

* SQLite is **serverless** and **embedded** in the application.
* Each database is a **single file** (`.sqlite`, `.db`, `.sqlite3`).
* Typically stored inside the **project folder** or application data directory.

**Example paths:**

* Linux: `/home/user/project/db/mydb.sqlite`
* Windows: `C:\Projects\MyApp\data\mydb.sqlite`
* macOS: `/Users/user/project/db/mydb.sqlite`

> ⚠️ There is no standalone SQLite installation; the DB file is part of the project.

---

## 🔹 2. Configuration

* SQLite does **not have a global config file**.
* Settings are applied **per connection** using PRAGMA commands:

```sql
PRAGMA foreign_keys = ON;
PRAGMA journal_mode = WAL;
PRAGMA synchronous = FULL;
```

---

## 🔹 3. Logging / Auditing

SQLite does **not log queries by default**. Logging can be implemented through:

1. **Application-level logging**

   * Most frameworks and libraries (Python, C#, Java) log queries from the code.

2. **Database triggers**

   * Track changes on important tables:

   ```sql
   CREATE TABLE audit_log (
       action TEXT,
       table_name TEXT,
       old_data TEXT,
       new_data TEXT,
       changed_at DATETIME DEFAULT CURRENT_TIMESTAMP
   );

   CREATE TRIGGER log_update
   AFTER UPDATE ON my_table
   BEGIN
       INSERT INTO audit_log(action, table_name, old_data, new_data)
       VALUES('UPDATE', 'my_table', OLD.col1 || ',' || OLD.col2, NEW.col1 || ',' || NEW.col2);
   END;
   ```

3. **CLI logging** (optional)

   ```bash
   sqlite3 mydb.sqlite ".log stdout"
   ```

   * Captures all commands entered interactively.

---

## 🔹 4. Per-Database / Per-User Logging

* SQLite is **embedded**, so there is no concept of per-user or per-database logging.
* Auditing must be implemented at the **table level**, **trigger level**, or in the **application code**.

---

## 🔹 5. Best Practices

* Keep the SQLite database file inside the project folder or a controlled data folder.
* Use **triggers** for auditing important tables instead of logging everything.
* Avoid generating massive logs inside the database file; use external logging if needed.
* Each project/application manages its own database file; there is no shared/global SQLite instance.

---

✅ **Key Standard:**

* SQLite = always **embedded** in the application.
* Each project has its **own database file**.
* No distinction between standard vs bundled.
* Logging is implemented **per database file** using triggers or application code.
