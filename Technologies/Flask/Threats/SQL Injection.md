# 🐍 **Flask – SQL Injection (SQLi) Sinks**

---

## 🔹 1. Raw SQL with SQLite / psycopg2 / MySQLdb

* `cursor.execute(sql)`
* `cursor.executemany(sql)`

❌ Unsafe if concatenating user input.

---

## 🔹 2. SQLAlchemy Core / ORM

* `engine.execute(sql)`
* `session.execute(sql)`
* `connection.execute(sql)`

⚠️ Passing user input into raw SQL strings = SQLi.

---

## 🔹 3. ORM Dangerous Patterns

* `session.query(...).from_statement(sql)`
* `text(sql)` (from `sqlalchemy.sql`) if interpolated with user input.
* `filter("id=" + user_input)` instead of parameters.

---

## 🔹 4. Peewee ORM (if used with Flask)

* `Model.raw(sql)` → vulnerable when concatenating.

---

## ⚠️ **Red Flags in Reviews**

* String concatenation (`+`, `%`, `.format()`, f-strings).
* f-strings with user input in query.
* `"WHERE ... {}".format(user_input)`

✅ **Safe**: Always use bound parameters

```python
session.execute("SELECT * FROM users WHERE name = :name", {"name": user_input})
```
