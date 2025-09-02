For ASP.NET Core, we configure the database in **`appsettings.json`** (and sometimes in `Program.cs` or `Startup.cs`).

Here’s the **ASP.NET Core – Database (SQL) Configuration**.

---

# 📒 ASP.NET Core – Database (SQL) Configuration

---

## 🔹 1. Location

* Main file: **`appsettings.json`**
* Extra setup: `Program.cs` (or `Startup.cs` in older versions)

---

## 🔹 2. Default Example (SQLite)

```json
// appsettings.json
{
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=app.db"
  }
}
```

In `Program.cs`:

```csharp
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlite(builder.Configuration.GetConnectionString("DefaultConnection")));
```

---

## 🔹 3. SQL Server Example

```json
// appsettings.json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=MyDatabase;User Id=myuser;Password=mypassword;"
  }
}
```

`Program.cs`:

```csharp
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));
```

---

## 🔹 4. PostgreSQL Example

```json
// appsettings.json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=localhost;Port=5432;Database=mydatabase;Username=myuser;Password=mypassword"
  }
}
```

`Program.cs`:

```csharp
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseNpgsql(builder.Configuration.GetConnectionString("DefaultConnection")));
```

---

## 🔹 5. MySQL / MariaDB Example

```json
// appsettings.json
{
  "ConnectionStrings": {
    "DefaultConnection": "server=127.0.0.1;port=3306;database=mydatabase;user=myuser;password=mypassword"
  }
}
```

`Program.cs`:

```csharp
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseMySql(builder.Configuration.GetConnectionString("DefaultConnection"),
        new MySqlServerVersion(new Version(8, 0, 28))));
```

---

## 🔹 6. Oracle Example

```json
// appsettings.json
{
  "ConnectionStrings": {
    "DefaultConnection": "User Id=myuser;Password=mypassword;Data Source=localhost:1521/xe"
  }
}
```

`Program.cs`:

```csharp
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseOracle(builder.Configuration.GetConnectionString("DefaultConnection")));
```

---

## 🔹 7. Remote DB with SSL Example

```json
// appsettings.json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=remotehost;Port=5432;Database=mydatabase;Username=myuser;Password=mypassword;SSL Mode=Require;Trust Server Certificate=true;"
  }
}
```

*Extra: SSL keywords differ by DB provider*

* **PostgreSQL:** `SSL Mode=Require;Trust Server Certificate=true;`
* **MySQL:** `SslMode=Required;`
* **SQL Server:** `Encrypt=True;TrustServerCertificate=False;`
* **Oracle:** Use `TCPS` in connection descriptor.

---

## 🔹 8. Notes

* The **connection string format** depends on the provider:

  * SQLite → `Data Source=file.db`
  * SQL Server → `Server=...;Database=...;User Id=...;Password=...;`
  * PostgreSQL → `Host=...;Database=...;Username=...;Password=...;`
  * MySQL/MariaDB → `server=...;database=...;user=...;password=...;`
  * Oracle → `User Id=...;Password=...;Data Source=...`

* Required NuGet packages:

  * SQL Server → `Microsoft.EntityFrameworkCore.SqlServer`
  * PostgreSQL → `Npgsql.EntityFrameworkCore.PostgreSQL`
  * MySQL/MariaDB → `Pomelo.EntityFrameworkCore.MySql`
  * Oracle → `Oracle.EntityFrameworkCore`
  * SQLite → `Microsoft.EntityFrameworkCore.Sqlite`

* Initialize DB context:

  ```csharp
  public class AppDbContext : DbContext
  {
      public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }

      public DbSet<User> Users { get; set; }
  }
  ```

* Apply migrations:

  ```bash
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```
