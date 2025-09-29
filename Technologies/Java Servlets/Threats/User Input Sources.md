# ☕ **Java Servlet – User Input Sources**

---

## 🔹 1. Query String (GET Parameters)

```java
// Example: GET /search?name=admin
@WebServlet("/search")
public class SearchServlet extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        String name = request.getParameter("name");
        response.getWriter().println("Searching for: " + name);
    }
}
```

---

## 🔹 2. Route Parameters (Path Info)

```java
// Example: GET /users/42 (42 is path info)
@WebServlet("/users/*")
public class UserServlet extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        String id = request.getPathInfo().substring(1); // "42"
        response.getWriter().println("User ID = " + id);
    }
}
```

---

## 🔹 3. Form Data (POST Parameters)

```java
// Example: POST /login with form fields
@WebServlet("/login")
public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException {
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        response.getWriter().println("User: " + username);
    }
}
```

---

## 🔹 4. JSON Body

```java
// Example: POST /api with JSON: { "email": "test@example.com" }
@WebServlet("/api")
public class ApiServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException {
        String body = request.getReader().lines().collect(Collectors.joining(System.lineSeparator()));
        response.getWriter().println("JSON Body: " + body);
    }
}
```

---

## 🔹 5. File Upload (Single & Multiple)

```java
// Single file upload
@WebServlet("/upload")
@MultipartConfig
public class UploadServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
        Part file = request.getPart("file");
        response.getWriter().println("Uploaded: " + file.getSubmittedFileName());
    }
}

// Multiple files upload
@WebServlet("/upload-multiple")
@MultipartConfig
public class UploadMultiServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
        for (Part file : request.getParts()) {
            response.getWriter().println("File: " + file.getSubmittedFileName());
        }
    }
}
```

---

## 🔹 6. Headers

```java
@WebServlet("/header")
public class HeaderServlet extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        String userAgent = request.getHeader("User-Agent");
        response.getWriter().println("User-Agent: " + userAgent);
    }
}
```

---

## 🔹 7. Cookies

```java
@WebServlet("/cookie")
public class CookieServlet extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        Cookie[] cookies = request.getCookies();
        String sessionId = "none";
        if (cookies != null) {
            for (Cookie c : cookies) {
                if (c.getName().equals("session_id")) {
                    sessionId = c.getValue();
                }
            }
        }
        response.getWriter().println("Session ID: " + sessionId);
    }
}
```

---

## 🔹 8. Session Data

```java
@WebServlet("/session")
public class SessionServlet extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        HttpSession session = request.getSession();
        session.setAttribute("user", "admin");
        response.getWriter().println("Session user = " + session.getAttribute("user"));
    }
}
```

---

## 🔹 9. Raw Request Body

```java
@WebServlet("/raw")
public class RawServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException {
        String rawBody = request.getReader().lines().collect(Collectors.joining(System.lineSeparator()));
        response.getWriter().println(rawBody);
    }
}
```

---

## 🔹 10. All Inputs at Once

```java
@WebServlet("/all")
public class AllInputsServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException {
        Map<String, String[]> params = request.getParameterMap();
        response.getWriter().println(params.toString());
    }
}
```
