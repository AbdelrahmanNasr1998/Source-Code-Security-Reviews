# ⚙️ **ASP.NET Core – SQL Injection (SQLi) Sinks**

---

## 🔹 1. Raw SQL Execution (Entity Framework Core)

* `context.Database.ExecuteSqlRaw(sql)`
* `context.Database.ExecuteSqlInterpolated(sql)`
* `context.Set<TEntity>().FromSqlRaw(sql)`
* `context.Set<TEntity>().FromSqlInterpolated(sql)`

⚠️ Dangerous if concatenating strings with user input.

---

## 🔹 2. ADO.NET (Classic DB Access)

* `SqlCommand cmd = new SqlCommand(sql, connection)`

  * `cmd.ExecuteReader()`
  * `cmd.ExecuteNonQuery()`
  * `cmd.ExecuteScalar()`

❌ Vulnerable if `sql` is built from user input.

---

## 🔹 3. Dapper (Micro ORM)

* `connection.Query(sql)`
* `connection.Execute(sql)`

⚠️ Unsafe when concatenating raw user input.

---

## 🔹 4. Red Flags in Code Reviews

* `string query = "SELECT * FROM Users WHERE name = '" + userInput + "'"`
* String interpolation: `$"SELECT ... {userInput}"`
* `string.Format("SELECT ... {0}", userInput)`

---

## ✅ Safe Pattern

Use **parameterized queries**:

```csharp
cmd.Parameters.AddWithValue("@name", userInput);
```

or EF Core LINQ:

```csharp
var user = context.Users.Where(u => u.Name == userInput);
```
