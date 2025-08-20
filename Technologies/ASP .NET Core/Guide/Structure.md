## 1. 🗂️ **Top-Level Project Folder**

Created when you run:

```bash
dotnet new mvc -n ProjectName
```

Contains:

* **`ProjectName.csproj`** → Project file (dependencies, build config).

* **`Program.cs`** → Main entry point → starts the web host.

* **`Startup.cs`** *(in older versions < .NET 6)* → Configures services & middleware (moved into `Program.cs` in .NET 6+).

---

## 2. 📁 **Folders & Responsibilities**

### 🕹️ **Controllers**

* Handle incoming HTTP requests.

* Contain **action methods** that return responses (HTML, JSON, etc.).

* Example: `HomeController.cs`.

### 📦 **Models**

* Represent application **data** & **business objects**.

* Usually plain C# classes (POCOs).

* Often connected to **Entity Framework Core** (EF Core) for DB.

### 🖼️ **Views**

* Store `.cshtml` Razor templates.

* Each controller typically has a folder inside `Views/`.

* Example: `Views/Home/Index.cshtml`.

### 🌐 **wwwroot**

* Public web root for static files (CSS, JS, images).

* Example:

  ```
  wwwroot/
    css/
    js/
    images/
  ```

### ⚙️ **Properties**

* Contains `launchSettings.json` → defines environment configs (profiles for IIS, Kestrel, etc.).

### 🗄️ **Migrations**

* Created when using **Entity Framework Core** with DB migrations.

* Stores schema changes.

### 🧩 **Data** *(optional, by convention)*

* Contains `DbContext` classes and data access logic.

### 🔧 **Services** *(best practice, not default)*

* Business logic layer (separates logic from controllers).

### 📤 **DTOs (Data Transfer Objects)** *(optional)*

* Used to transfer data between layers (especially in APIs).

---

## 3. 📋 **Other Common Files**

* **`appsettings.json`** → App configuration (connection strings, logging, etc.).

* **`appsettings.Development.json`** → Environment-specific config.

* **`_ViewImports.cshtml`** → Common namespaces & tag helpers for Razor views.

* **`_ViewStart.cshtml`** → Layout setup for views.

---

## 4. 🌳 **Project Tree**

```
ProjectName/
│
├── ProjectName.csproj        # Project configuration & dependencies
├── Program.cs                # Main entry point (configures host, services, middleware)
├── Startup.cs                # (Pre .NET 6) Service & middleware setup
│
├── Controllers/              # Handle requests & return responses
│   └── HomeController.cs     # Example controller
│
├── Models/                   # Data models / business entities
│   └── User.cs               # Example model class
│
├── Views/                    # Razor view templates
│   ├── Home/
│   │   └── Index.cshtml      # Example view
│   ├── Shared/
│   │   └── _Layout.cshtml    # Master layout
│   └── _ViewImports.cshtml   # Shared namespaces & tag helpers
│
├── wwwroot/                  # Public static files
│   ├── css/                  # Stylesheets
│   ├── js/                   # JavaScript
│   └── images/               # Images
│
├── appsettings.json          # Main app configuration
├── appsettings.Development.json # Dev environment config
│
├── Properties/               
│   └── launchSettings.json   # Launch profiles (IIS, Kestrel, etc.)
│
├── Migrations/               # EF Core database migrations
│
├── Data/                     # (Optional) EF DbContext & repositories
│   └── AppDbContext.cs
│
├── Services/                 # (Optional) Business logic
│   └── UserService.cs
│
└── DTOs/                     # (Optional) Data transfer objects
    └── UserDto.cs
```

---

## 5. 🔄 **How It All Connects (Flow)**

```
User → sends HTTP request (e.g., /home/index)
      ↓
Program.cs → configures middleware & routes
      ↓
Routing → matches Controller & Action (HomeController → Index)
      ↓
Controller → may call Services layer (business logic)
      ↓
Services → may call Data/DbContext (database access)
      ↓
Model → represents data (fetched/saved with EF Core)
      ↓
Controller → passes data (Model or DTO) to View
      ↓
View (Razor .cshtml) → renders HTML with model data
      ↓
Response → sent back to User
```
