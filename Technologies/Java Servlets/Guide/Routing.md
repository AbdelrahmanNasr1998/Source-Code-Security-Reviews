# 🌱 Java Servlets & JSP – Project Structure & Routing

```
myapp/
│── src/main/java/com/example/myapp/
│   ├── controller/
│   │   └── HomeServlet.java      📂 # Servlets: handle routes (doGet/doPost)
│   ├── model/
│   │   └── User.java             🧩 # Models / POJOs
│   ├── dao/
│   │   └── UserDao.java          💾 # Data Access Layer (JDBC)
│   └── util/
│       └── DBUtil.java           ⚙️ # Database utility class
│
└── src/main/webapp/
    ├── index.jsp                 📝 # JSP views (can be homepage)
    └── WEB-INF/
        ├── web.xml               ⚙️ # Deployment descriptor (routes)
        └── views/
            └── users.jsp         📝 # JSP templates (protected)
```

---

## 🛣️ Routing Flow in Servlets & JSP

1. **Servlet Entry (web.xml or Annotation)**

Example with annotation:

```java
@WebServlet("/home")
public class HomeServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        resp.getWriter().println("Hello from Servlet!");
    }
}
```

* 🚀 Runs on an external **Tomcat/Jetty server** (e.g., `http://localhost:8080/myapp/`).

* 🔍 Handles requests directly with `doGet()`, `doPost()`, etc.

---

2. **Servlet with JSP Forwarding**

Example `HomeServlet.java`

```java
@WebServlet("/")
public class HomeServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {

        req.setAttribute("message", "Hello from JSP + Servlet!");
        RequestDispatcher dispatcher = req.getRequestDispatcher("index.jsp");
        dispatcher.forward(req, resp);
    }
}
```

---

3. **UserServlet (Basic CRUD Routing)**

```java
@WebServlet("/users")
public class UserServlet extends HttpServlet {
    private UserDao userDao = new UserDao();

    // GET http://localhost:8080/myapp/users
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        try {
            List<User> users = userDao.getAllUsers();
            req.setAttribute("users", users);
            req.getRequestDispatcher("WEB-INF/views/users.jsp").forward(req, resp);
        } catch (Exception e) {
            throw new ServletException(e);
        }
    }

    // POST http://localhost:8080/myapp/users
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        try {
            String name = req.getParameter("name");
            String email = req.getParameter("email");
            userDao.insertUser(new User(0, name, email));
            resp.sendRedirect("users");
        } catch (Exception e) {
            throw new ServletException(e);
        }
    }
}
```

---

4. **JSP View Example (users.jsp)**

```jsp
<%@ page contentType="text/html;charset=UTF-8" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<html>
<head><title>Users</title></head>
<body>
    <h2>User List</h2>
    <form action="users" method="post">
        Name: <input type="text" name="name" />
        Email: <input type="text" name="email" />
        <button type="submit">Add User</button>
    </form>
    <ul>
        <c:forEach var="u" items="${users}">
            <li>${u.name} - ${u.email}</li>
        </c:forEach>
    </ul>
</body>
</html>
```

---

## 🔄 Flow Summary

`🌐 Browser URL → 🟢 Tomcat (servlet container) → 📂 Servlet (doGet/doPost) → 🧩 Model/DAO (JDBC) → 📝 JSP (forwarded via RequestDispatcher)`

---

## ⚠️ Special Notes for Servlets & JSP Routing

* **Routing is done via:**

  * `@WebServlet("/path")` annotation
  * OR **`web.xml` mappings**:

    ```xml
    <servlet>
      <servlet-name>UserServlet</servlet-name>
      <servlet-class>com.example.myapp.controller.UserServlet</servlet-class>
    </servlet>
    <servlet-mapping>
      <servlet-name>UserServlet</servlet-name>
      <url-pattern>/users</url-pattern>
    </servlet-mapping>
    ```

* **Methods:**

  * `doGet()` → Handles GET 🟢
  * `doPost()` → Handles POST ✉️
  * `doPut()` → Handles PUT 🔄 (rare, must override)
  * `doDelete()` → Handles DELETE ❌ (rare, must override)

* **Parameters:**

  * `@PathVariable` equivalent → Use `req.getPathInfo()`
  * `@RequestParam` equivalent → Use `req.getParameter("name")`
