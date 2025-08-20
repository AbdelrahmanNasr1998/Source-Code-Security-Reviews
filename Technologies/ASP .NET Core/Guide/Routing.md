# ⚡ .NET Core Project Structure & Routing

```
MyApp/
│── Program.cs                 # 🚀 Entry point, configures app & middleware
│── Startup.cs (old versions)  # ⚙️ Used before .NET 6 for configuring services/pipeline
│
├── Controllers/
│   └── HomeController.cs      # 🧩 Controller: holds functions (Actions) for URLs
│
├── Models/
│   └── User.cs                # 📦 Models: represent data / entities
│
├── Views/
│   └── Home/
│       └── Index.cshtml       # 🖼️ View template rendered by controller
│
├── appsettings.json           # 🔧 Config (DB, secrets, etc.)
└── MyApp.csproj               # 📁 Project file
```

---

## 🛣️ Routing Flow in .NET Core

### 1. 📂 Program.cs (or Startup.cs in older versions)

* Configures **routing middleware**.

**Example (minimal hosting model in .NET 6+):**

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");

app.Run();
```

This means:

* Default controller = `Home` 🏠
* Default action = `Index` 📝
* `id` is optional 🔹

---

### 2. 🌐 Request → Controller

* User requests: `http://localhost:5000/`
* Matches default route → goes to `HomeController.Index()`

**Example controller (`Controllers/HomeController.cs`):**

```csharp
using Microsoft.AspNetCore.Mvc;

public class HomeController : Controller
{
    public IActionResult Index() // http://localhost:5000/home/index
    {
        return View(); // 🖼️ Renders Views/Home/Index.cshtml
    }

    public IActionResult About() // http://localhost:5000/home/about
    {
        return Content("About Page");
    }
}
```

---

### 3. 🖼️ Controller → View

* `return View();` looks for a `.cshtml` file in `/Views/{ControllerName}/{ActionName}.cshtml`

**Example:**
`HomeController.Index()` → `/Views/Home/Index.cshtml`

```html
<h1>Hello from Home/Index</h1>
```

---

## 🔄 Flow Summary

`🌐 Browser URL → 📂 Program.cs Routing → 🧩 Controller (Action Method) → 🖼️ (optional) View (.cshtml)`
