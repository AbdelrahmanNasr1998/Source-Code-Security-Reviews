# 🌱 Spring Boot Project Structure & Routing

```
myapp/
│── src/main/java/com/example/myapp/
│   ├── MyAppApplication.java     🟢 # Entry point (Spring Boot starter)
│   ├── controller/
│   │   └── HomeController.java   📂 # Controllers: hold route mappings
│   ├── model/
│   │   └── User.java             🧩 # Models / Entities
│   ├── service/
│   │   └── UserService.java      ⚙️ # Services (business logic)
│   └── repository/
│       └── UserRepository.java   💾 # Data Access Layer (JPA, DB access)
│
└── src/main/resources/
    └── templates/
        └── index.html            📝 # Thymeleaf / Freemarker / JSP views
```

---

## 🛣️ Routing Flow in Spring Boot

1. **Main Application**

```java
@SpringBootApplication
public class MyAppApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyAppApplication.class, args);
    }
}
```

* 🚀 Starts embedded **Tomcat server** (default `http://localhost:8080`)

* 🔍 Scans packages for controllers, services, etc.

---

2. **Controller with Routes**

Example `HomeController.java`

```java
package com.example.myapp.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

@Controller
public class HomeController {

    // GET http://localhost:8080/
    @GetMapping("/")
    public String index() {
        return "index"; // Renders templates/index.html
    }

    // GET http://localhost:8080/about
    @GetMapping("/about")
    @ResponseBody // returns text instead of template
    public String about() {
        return "About Page";
    }
}
```

---

3. **REST API Controller**

Example `UserController.java`

```java
package com.example.myapp.controller;

import org.springframework.web.bind.annotation.*;
import java.util.*;

@RestController   // returns JSON by default
@RequestMapping("/users") // Base path for this controller
public class UserController {

    // GET http://localhost:8080/users
    @GetMapping
    public List<String> getUsers() {
        return Arrays.asList("Alice", "Bob");
    }

    // GET http://localhost:8080/users/5
    @GetMapping("/{id}")
    public String getUser(@PathVariable int id) {
        return "User ID: " + id;
    }

    // POST http://localhost:8080/users
    @PostMapping
    public String createUser(@RequestBody String user) {
        return "User created: " + user;
    }
}
```

---

4. **Templates (Thymeleaf Example)**

`src/main/resources/templates/index.html`

```html
<!DOCTYPE html>
<html>
<head><title>Home</title></head>
<body>
  <h1>Hello from Thymeleaf!</h1>
</body>
</html>
```

---

## 🔄 Flow Summary

`🌐 Browser URL → 🟢 Embedded Tomcat → 🧩 DispatcherServlet → 📂 Controller (with @GetMapping/@PostMapping/etc.) → 📝 (optional) Template Engine`

---

## ⚠️ Special Notes for Spring Boot Routing

* **Annotations for Routing:**

  * `@GetMapping("/path")` → Handles `GET` 🟢

  * `@PostMapping("/path")` → Handles `POST` ✉️

  * `@PutMapping("/path")` → Handles `PUT` 🔄

  * `@DeleteMapping("/path")` → Handles `DELETE` ❌

  * `@RequestMapping("/path")` → Can handle all, or specify method 🔧
* **Return Types:**

  * `@Controller` + `return "viewName"` → Loads HTML template 📝

  * `@RestController` or `@ResponseBody` → Returns JSON/text directly 💬
* **Path Variables & Query Params:**

```java
@GetMapping("/users/{id}")   // http://localhost:8080/users/10
public String getUser(@PathVariable int id) { ... }

@GetMapping("/search")       // http://localhost:8080/search?name=Alice
public String search(@RequestParam String name) { ... }
```

* **Base Path for Controllers:**

```java
@RequestMapping("/api")  // All routes inside start with /api 🛤️
```
