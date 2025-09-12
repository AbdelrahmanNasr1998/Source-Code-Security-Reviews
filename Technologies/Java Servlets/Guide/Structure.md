## 1. 🗂️ **Top-Level Project Folder**

Created when you generate a **Dynamic Web Project** (Eclipse/IntelliJ) or with **Maven/Gradle**.

Contains:

* **`pom.xml`** (Maven) → Dependency management (e.g., Servlet API, JSP, JSTL).
* **`src/`** → Java source code.
* **`WebContent/`** (or `src/main/webapp/` for Maven) → Web resources.
* **`WEB-INF/`** → Deployment descriptor, libraries, and private files.
* **`target/`** → Compiled build output (ignored in Git).

---

## 2. 📁 **Folders & Responsibilities**

### 🟢 `src/main/java/com/example/project/`

* **Servlet classes** → Extend `HttpServlet`.
* Handle HTTP requests via `doGet()` / `doPost()` methods.
* Example:

  ```java
  @WebServlet("/hello")
  public class HelloServlet extends HttpServlet {
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
          resp.getWriter().println("Hello from Servlet!");
      }
  }
  ```

---

### 🕹️ `controller/`

* Not always a separate folder in small projects, but good practice.
* Contains servlets that process requests.
* Example: `UserServlet.java` → `/users`.

---

### 📄 `WebContent/` (or `src/main/webapp/`)

* Public-facing files.
* Contains HTML, CSS, JS, JSP pages.

---

### 🔐 `WEB-INF/`

* **`web.xml`** → Deployment descriptor (if annotations are not used).
* **`lib/`** → JAR dependencies (Servlet API, JSTL, DB drivers).
* **`views/`** → JSP pages (not directly accessible by URL).
* Example: `WEB-INF/views/user.jsp`.

---

### 🗄️ `model/`

* JavaBeans or POJOs (Plain Old Java Objects).
* Represent data entities.
* Example:

  ```java
  public class User {
      private int id;
      private String name;
      // getters & setters
  }
  ```

---

### 🛠️ `dao/`

* Data Access Objects.
* Handle DB interaction (JDBC).
* Example:

  ```java
  public class UserDao {
      public List<User> getAllUsers() throws SQLException {
          Connection conn = DBUtil.getConnection();
          Statement stmt = conn.createStatement();
          ResultSet rs = stmt.executeQuery("SELECT * FROM users");
          ...
      }
  }
  ```

---

### ⚙️ `util/`

* Utility classes (DB connection, helpers).
* Example: `DBUtil.java`.

---

## 3. 📂 **Other Files**

* **`index.jsp`** → Default home page.
* **`style.css`** → Inside `WebContent/css/`.
* **`script.js`** → Inside `WebContent/js/`.
* **`web.xml`** → Maps URLs to servlets (if not using `@WebServlet`).

  ```xml
  <servlet>
      <servlet-name>UserServlet</servlet-name>
      <servlet-class>com.example.controller.UserServlet</servlet-class>
  </servlet>
  <servlet-mapping>
      <servlet-name>UserServlet</servlet-name>
      <url-pattern>/users</url-pattern>
  </servlet-mapping>
  ```

---

## 4. 🌳 **Project Tree**

```
project-name/
│
├── pom.xml                      # Maven dependencies (servlet-api, jsp, jstl, mysql, etc.)
├── src/
│   ├── main/
│   │   ├── java/com/example/project/
│   │   │   ├── controller/        # Servlets
│   │   │   │   └── UserServlet.java
│   │   │   ├── model/             # Data models
│   │   │   │   └── User.java
│   │   │   ├── dao/               # Data access (JDBC)
│   │   │   │   └── UserDao.java
│   │   │   └── util/              # Utilities
│   │   │       └── DBUtil.java
│   │   │
│   │   └── webapp/ (or WebContent/)
│   │       ├── index.jsp          # Home page
│   │       ├── css/               # Stylesheets
│   │       │   └── style.css
│   │       ├── js/                # JavaScript files
│   │       │   └── script.js
│   │       └── WEB-INF/
│   │           ├── web.xml         # Deployment descriptor
│   │           ├── lib/            # External libraries
│   │           └── views/          # JSP files (not directly public)
│   │               └── user.jsp
│   │
│   └── test/java/com/example/project/
│       └── UserServletTest.java    # Unit tests (optional)
│
└── target/                        # Compiled build output
```

---

## 5. 🔄 **How It All Connects (Flow)**

```
User → sends HTTP request (/users)
      ↓
Servlet (UserServlet) → handles request via doGet/doPost
      ↓
DAO (UserDao) → fetch/save data from DB (JDBC)
      ↓
Model (User) → JavaBean holding data
      ↓
RequestDispatcher → forwards data to JSP (user.jsp)
      ↓
JSP (user.jsp) → renders dynamic HTML (with JSTL/EL)
      ↓
Response → sent back to User
```

---

✅ **Extra Notes (Servlets/JSP Only):**

* **JSP** = mixes HTML + Java (using JSTL/EL instead of scriptlets is best practice).
* **Servlets** = handle logic, not UI.
* **JSTL (JavaServer Pages Standard Tag Library)** is used for loops, conditions, etc. inside JSP.

  ```jsp
  <c:forEach var="user" items="${users}">
      <p>${user.name}</p>
  </c:forEach>
  ```
* Unlike Spring Boot, you **must configure `web.xml`** (unless you use `@WebServlet` annotations).
