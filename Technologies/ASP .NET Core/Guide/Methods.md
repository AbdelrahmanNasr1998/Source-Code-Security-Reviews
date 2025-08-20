## 🔹 1. Controllers & Actions Basics

In ASP.NET Core, you usually use a **Controller** with methods (`Action Methods`) mapped to HTTP verbs:

* `[HttpGet]` → GET
* `[HttpPost]` → POST
* `[HttpPut]` → PUT
* `[HttpPatch]` → PATCH
* `[HttpDelete]` → DELETE

👉 Parameters can come from:

* `FromQuery` → Query string (`?key=value`)
* `FromForm` → Form data
* `FromBody` → JSON body
* `FromRoute` → URL parameters (`/users/1`)
* `IFormFile` / `List<IFormFile>` → File uploads

---

## 🔹 2. GET Example

```csharp
using Microsoft.AspNetCore.Mvc;

[ApiController]
[Route("api/[controller]")]
public class ProfileController : ControllerBase
{
    // GET /api/profile/user/1   ← Route Parameter
    [HttpGet("user/{id}")]
    public IActionResult GetUserById([FromRoute] int id)
    {
        // "id" comes from the URL segment
        return Ok(new { method = "GET", source = "route", userId = id });
    }

    // GET /api/profile?user=1   ← Query String
    [HttpGet]
    public IActionResult GetUserByQuery([FromQuery] int user)
    {
        // "user" comes from the query string
        return Ok(new { method = "GET", source = "query", userId = user });
    }
}
```

---

## 🔹 3. POST Examples

### ✅ POST – Form Data

```csharp
// POST /api/user
[HttpPost("form")]
public IActionResult PostForm([FromForm] string name)
{
    return Ok(new { method = "POST", type = "form", name });
}
```

👉 Form body: `name=Ali`

---

### ✅ POST – JSON Body

```csharp
public class UserDto
{
    public string Name { get; set; }
}

// POST /api/user
[HttpPost("json")]
public IActionResult PostJson([FromBody] UserDto user)
{
    return Ok(new { method = "POST", type = "json", name = user.Name });
}
```

👉 JSON body:

```json
{ "name": "Sara" }
```

---

## 🔹 4. PUT Example

```csharp
public class UpdateUserDto
{
    public string Name { get; set; }
    public string City { get; set; }
}

// PUT /api/user
[HttpPut]
public IActionResult PutUser([FromBody] UpdateUserDto user)
{
    return Ok(new { method = "PUT", updatedUser = user });
}
```

👉 Used when **replacing a resource fully**.

---

## 🔹 5. PATCH Example (Partial Update)

```csharp
using Microsoft.AspNetCore.JsonPatch;

public class PatchUserDto
{
    public string Name { get; set; }
    public string City { get; set; }
}

// PATCH /api/user/1
[HttpPatch("{id}")]
public IActionResult PatchUser(int id, [FromBody] JsonPatchDocument<PatchUserDto> patchDoc)
{
    if (patchDoc == null) return BadRequest();

    var user = new PatchUserDto { Name = "Default", City = "Default" };
    patchDoc.ApplyTo(user); // Apply only the provided changes

    return Ok(new { method = "PATCH", id, updated = user });
}
```

👉 Example PATCH body:

```json
[
  { "op": "replace", "path": "/Name", "value": "Ali" }
]
```

---

## 🔹 6. DELETE Example

```csharp
// DELETE /api/user/1
[HttpDelete("{id}")]
public IActionResult DeleteUser(int id)
{
    return Ok(new { method = "DELETE", deletedUserId = id });
}
```

---

## 🔹 7. File Uploads

### ✅ Single File

```csharp
// POST /api/user/upload
[HttpPost("upload")]
public IActionResult UploadFile([FromForm] IFormFile file)
{
    if (file == null) return BadRequest("No file uploaded");

    return Ok(new { 
        method = "POST", 
        filename = file.FileName, 
        size = file.Length 
    });
}
```

---

### ✅ Multiple Files

```csharp
// POST /api/user/upload-multiple
[HttpPost("upload-multiple")]
public IActionResult UploadMultiple([FromForm] List<IFormFile> files)
{
    var fileInfos = files.Select(f => new { f.FileName, f.Length }).ToList();

    return Ok(new { 
        method = "POST", 
        uploadedFiles = fileInfos 
    });
}
```

👉 **Important**: For uploads, the HTML form or Postman must set
`enctype="multipart/form-data"`

---

# 📝 Summary

✅ **ASP.NET Core Request Sources**

* `[FromQuery]` → query string (`?key=value`)
* `[FromForm]` → form fields
* `[FromBody]` → JSON body
* `[FromRoute]` → URL parameter (`/users/1`)
* `IFormFile` / `List<IFormFile>` → single/multiple file uploads

✅ **Methods**

* **GET** → `[HttpGet]`
* **POST** → `[HttpPost]`
* **PUT** → `[HttpPut]` (replace resource)
* **PATCH** → `[HttpPatch]` (partial update with `JsonPatchDocument`)
* **DELETE** → `[HttpDelete]`

✅ **Files**

* Single → `IFormFile file`
* Multiple → `List<IFormFile> files`
