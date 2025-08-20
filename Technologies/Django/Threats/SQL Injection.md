# 🐍 **Django – SQL Injection (SQLi) Sinks**

---

## 🔹 1. Raw SQL Execution (DB Cursor)

* `cursor.execute(sql)` → vulnerable if string concatenation / f-strings.
* `cursor.executemany(sql)` → same risk.

---

## 🔹 2. ORM Raw Queries

* `Model.objects.raw(sql)` → user input directly inside raw string = SQLi.

---

## 🔹 3. QuerySet `.extra()` (Deprecated but still present)

* `QuerySet.extra(where=[sql])` → injecting user input here is dangerous.

---

## 🔹 4. Custom Manager / Raw SQL

* `Manager.raw(sql)` or similar → if built with user input.

---

## 🔹 5. Rare Cases

* `values_list().raw(sql)` or any method wrapping raw SQL with user input.

---

## ⚠️ **Red Flags in Reviews**

* String concatenation (`+`, `%`, `.format()`, f-strings).
* User input directly inside query strings.

✅ **Safe**: parameterized queries

```python
cursor.execute("SELECT * FROM users WHERE name = %s", [user_input])
```
