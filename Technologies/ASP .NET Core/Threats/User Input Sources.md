# ⚙️ **ASP.NET Core – User Input Sources**

---

## 🔹 **1. Query String (GET parameters)**

* Access via `Request.Query` (dictionary-like)

```csharp
// Example: GET /search?name=admin
string name = Request.Query["name"];
```

---

## 🔹 **2. Route Parameters (Path Variables)**

* Defined in route templates and auto-bound to method parameters.

```csharp
[HttpGet("user/{id}")]
public IActionResult GetUser(int id)
{
    return Ok($"User ID = {id}");
}
```

---

## 🔹 **3. Form Data (POST requests)**

* Access via `Request.Form` (form-encoded body)

```csharp
string username = Request.Form["username"];
```

---

## 🔹 **4. JSON Body (API Controllers)**

* Automatically bound to method parameters or models using `[FromBody]`.

```csharp
public class LoginDto { public string Email { get; set; } public string Password { get; set; } }

[HttpPost("login")]
public IActionResult Login([FromBody] LoginDto dto)
{
    return Ok(dto.Email);
}
```

* Raw body access:

```csharp
using (var reader = new StreamReader(Request.Body))
{
    string raw = await reader.ReadToEndAsync();
}
```

---

## 🔹 **5. File Uploads**

* Single file:

```csharp
[HttpPost("upload")]
public IActionResult Upload(IFormFile file)
{
    var filename = file.FileName;
    return Ok(filename);
}
```

* Multiple files:

```csharp
[HttpPost("upload-multi")]
public IActionResult Upload(List<IFormFile> files)
{
    return Ok(files.Count);
}
```

---

## 🔹 **6. Headers**

* Access via `Request.Headers`

```csharp
string ua = Request.Headers["User-Agent"];
string ip = Request.Headers["X-Forwarded-For"];
```

⚠️ Critical: `X-Forwarded-For`, `Host`, `Origin`, and `Referer` can be **spoofed and dangerous**.

---

## 🔹 **7. Cookies**

* Access via `Request.Cookies`

```csharp
string sessionId = Request.Cookies["session_id"];
```

* Response cookies: `Response.Cookies.Append("token", "abc");`

---

## 🔹 **8. Session Data (Server-side, but user-controlled indirectly)**

```csharp
HttpContext.Session.SetString("cart", "item1,item2");
string cart = HttpContext.Session.GetString("cart");
```

---

## 🔹 **9. Model Binding (Automatic User Input Mapping)**

ASP.NET Core automatically maps **query string, route params, form data, and JSON** into method parameters or model objects.

```csharp
public IActionResult Register(UserModel model) // auto-filled from user input
{
    return Ok(model.Username);
}
```

⚠️ **Security Risk:** Always validate models (`[Required]`, `[StringLength]`, FluentValidation, etc.).

---

## 🔹 **10. Other Input Sources**

* `Request.QueryString` → Full raw query string
* `Request.Body` → Raw request body stream
* `Request.Form.Files` → File upload collection
* `Request.Host`, `Request.Path`, `Request.Scheme` → attacker-controlled if behind proxy
* `Request.Headers["Authorization"]` → user-supplied credentials

---

# ✅ **Security Review Checklist (ASP.NET Core)**

When reviewing code, trace:

* `Request.Query` / route parameters
* `Request.Form`
* `[FromBody]` JSON models
* `Request.Headers` / `Request.Cookies`
* File uploads (`IFormFile`)
* Auto model binding

🚨 Dangerous if passed directly to:

* `Process.Start()`, `System.Diagnostics` → **Command Injection**
* `string.Format()` / `Console.WriteLine()` → **Format String bugs**
* ORM queries (`context.Users.FromSqlRaw(userInput)`) → **SQLi**
* Razor views with `@Html.Raw(userInput)` → **XSS**
