# ☕ **Spring Boot – User Input Sources**

---

## 🔹 1. Query String (GET Parameters)

```java
// Example: GET /search?name=admin
@GetMapping("/search")
public String search(@RequestParam String name) {
    return "Searching for: " + name;
}
```

---

## 🔹 2. Route Parameters (Path Variables)

```java
// Example: GET /users/42
@GetMapping("/users/{id}")
public String getUser(@PathVariable("id") String id) {
    return "User ID = " + id;
}
```

---

## 🔹 3. Form Data (POST Parameters)

```java
// Example: POST /login with form fields
@PostMapping("/login")
public String login(
    @RequestParam String username,
    @RequestParam String password
) {
    return "User: " + username;
}
```

---

## 🔹 4. JSON Body

```java
// Example: POST /api with JSON: { "email": "test@example.com" }
@PostMapping("/api")
public String create(@RequestBody Map<String, Object> payload) {
    return "Email: " + payload.get("email");
}
```

Or using DTO:

```java
class UserDTO {
    public String email;
    public String password;
}

@PostMapping("/register")
public String register(@RequestBody UserDTO user) {
    return "Email: " + user.email;
}
```

---

## 🔹 5. File Upload (Single & Multiple)

```java
// Single file upload
@PostMapping("/upload")
public String upload(@RequestParam("file") MultipartFile file) throws IOException {
    return "Uploaded: " + file.getOriginalFilename();
}

// Multiple files upload
@PostMapping("/upload-multiple")
public String uploadMulti(@RequestParam("files") MultipartFile[] files) {
    for (MultipartFile file : files) {
        System.out.println(file.getOriginalFilename());
    }
    return "Uploaded multiple files!";
}
```

---

## 🔹 6. Headers

```java
@GetMapping("/header")
public String header(@RequestHeader("User-Agent") String userAgent) {
    return "User-Agent: " + userAgent;
}
```

---

## 🔹 7. Cookies

```java
@GetMapping("/cookie")
public String cookie(@CookieValue(value = "session_id", defaultValue = "none") String sessionId) {
    return "Session ID: " + sessionId;
}
```

---

## 🔹 8. Session Data

```java
@GetMapping("/session")
public String session(HttpSession session) {
    session.setAttribute("user", "admin");
    return "Session user = " + session.getAttribute("user");
}
```

---

## 🔹 9. Raw Request Body

```java
@PostMapping("/raw")
public String raw(HttpServletRequest request) throws IOException {
    String rawBody = request.getReader().lines().collect(Collectors.joining(System.lineSeparator()));
    return rawBody;
}
```

---

## 🔹 10. All Inputs at Once

```java
@PostMapping("/all")
public String allInputs(HttpServletRequest request) {
    Map<String, String[]> params = request.getParameterMap();
    return params.toString();
}
```
