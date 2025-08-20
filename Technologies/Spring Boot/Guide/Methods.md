# 📒 Spring Boot – Request Methods & File Uploads

---

## 🔹 1. GET Example

### ✅ GET – Query Parameter

```java
@RestController
@RequestMapping("/api/user")
public class UserController {

    // GET /api/user?city=Cairo
    @GetMapping
    public Map<String, Object> getUser(@RequestParam(defaultValue = "Guest") String city) {
        // @RequestParam → query string
        Map<String, Object> response = new HashMap<>();
        response.put("method", "GET");
        response.put("city", city);
        return response;
    }
```

### ✅ GET – Path Variable

```java
    // GET /api/user/123
    @GetMapping("/{id}")
    public Map<String, Object> getUserById(@PathVariable int id) {
        // @PathVariable → route parameter
        Map<String, Object> response = new HashMap<>();
        response.put("method", "GET");
        response.put("id", id);
        return response;
    }
```

---

## 🔹 2. POST Example

### ✅ POST – JSON

```java
public static class UserDto {
    public String name;
}

@PostMapping("/json")
public Map<String, Object> postJson(@RequestBody UserDto user) {
    // @RequestBody → automatically maps JSON to object
    Map<String, Object> response = new HashMap<>();
    response.put("method", "POST");
    response.put("name", user.name);
    return response;
}
```

### ✅ POST – Form Data

```java
@PostMapping("/form")
public Map<String, Object> postForm(@RequestParam String name) {
    // @RequestParam also works for form-data
    Map<String, Object> response = new HashMap<>();
    response.put("method", "POST");
    response.put("name", name);
    return response;
}
```

---

## 🔹 3. PUT Example

```java
@PutMapping("/{id}")
public Map<String, Object> putUser(@PathVariable int id, @RequestBody UserDto user) {
    Map<String, Object> response = new HashMap<>();
    response.put("method", "PUT");
    response.put("id", id);
    response.put("updatedUser", user);
    return response;
}
```

👉 Used for **full replacement** of a resource.

---

## 🔹 4. PATCH Example (Partial Update)

```java
@PatchMapping("/{id}")
public Map<String, Object> patchUser(@PathVariable int id, @RequestBody Map<String, Object> updates) {
    // Map contains only fields to update
    Map<String, Object> response = new HashMap<>();
    response.put("method", "PATCH");
    response.put("id", id);
    response.put("updatedFields", updates);
    return response;
}
```

---

## 🔹 5. DELETE Example

```java
@DeleteMapping("/{id}")
public Map<String, Object> deleteUser(@PathVariable int id) {
    Map<String, Object> response = new HashMap<>();
    response.put("method", "DELETE");
    response.put("deletedUserId", id);
    return response;
}
```

---

## 🔹 6. File Uploads

### ✅ Single File

```java
@PostMapping("/upload-single")
public Map<String, Object> uploadSingle(@RequestParam("file") MultipartFile file) throws IOException {
    Map<String, Object> response = new HashMap<>();
    response.put("method", "POST");
    response.put("filename", file.getOriginalFilename());
    response.put("size", file.getSize());
    return response;
}
```

### ✅ Multiple Files

```java
@PostMapping("/upload-multiple")
public Map<String, Object> uploadMultiple(@RequestParam("files") MultipartFile[] files) throws IOException {
    List<Map<String, Object>> fileInfos = Arrays.stream(files)
        .map(f -> Map.of("name", f.getOriginalFilename(), "size", f.getSize()))
        .toList();

    Map<String, Object> response = new HashMap<>();
    response.put("method", "POST");
    response.put("uploadedFiles", fileInfos);
    return response;
}
```

👉 **Important:** In HTML form or Postman, set `enctype="multipart/form-data"`

---

## 📝 Summary

✅ **Request Sources**

* `@RequestParam` → query string or form data
* `@PathVariable` → route parameter (`/user/{id}`)
* `@RequestBody` → JSON body → automatically deserialized to object
* `MultipartFile` / `MultipartFile[]` → single/multiple file uploads

✅ **HTTP Methods**

* GET → `@GetMapping`
* POST → `@PostMapping`
* PUT → `@PutMapping` (replace resource)
* PATCH → `@PatchMapping` (partial update)
* DELETE → `@DeleteMapping`

✅ **Files**

* Single → `@RequestParam("file") MultipartFile file`
* Multiple → `@RequestParam("files") MultipartFile[] files`
