# ☕ **Java Servlet – SQL Injection (SQLi) Sinks**

---

## 🔹 1. JDBC (Raw SQL in Servlet)

* ```java
  // vulnerable: concatenation with request parameter
  String id = request.getParameter("id");
  Statement stmt = conn.createStatement();
  ResultSet rs = stmt.executeQuery("SELECT * FROM users WHERE id = " + id);
  ```
* ```java
  // vulnerable: string concatenation inside a servlet
  String name = request.getParameter("name");
  stmt.executeQuery("SELECT * FROM users WHERE name = '" + name + "'");
  ```

⚠️ Direct concatenation of `HttpServletRequest` data into `Statement` = **classic SQLi**.

---

## 🔹 2. DataSource / Connection + Servlet Helpers

* ```java
  // vulnerable if concatenated
  String role = request.getParameter("role");
  try (Connection conn = dataSource.getConnection();
       Statement s = conn.createStatement()) {
      s.executeUpdate("UPDATE users SET role = '" + role + "' WHERE id = " + request.getParameter("id"));
  }
  ```

⚠️ Any `Connection` used in a servlet with string-built SQL is vulnerable.

---

## 🔹 3. PreparedStatement misuse vs correct use

* **Vulnerable (misuse — still concatenating):**

  ```java
  String sql = "SELECT * FROM users WHERE email = '" + request.getParameter("email") + "'";
  PreparedStatement ps = conn.prepareStatement(sql); // no parameter binding
  ps.executeQuery();
  ```
* **Safe (proper PreparedStatement):**

  ```java
  PreparedStatement ps = conn.prepareStatement("SELECT * FROM users WHERE email = ?");
  ps.setString(1, request.getParameter("email"));
  ps.executeQuery();
  ```

✅ Always use placeholders and set methods — do **not** build the SQL string from user input.

---

## 🔹 4. Using ORMs / JPA / Hibernate from Servlets

* **JPQL / HQL vulnerable when concatenated**:

  ```java
  String name = request.getParameter("name");
  List<User> users = em.createQuery("FROM User u WHERE u.name = '" + name + "'", User.class).getResultList();
  ```
* **Native query vulnerable when concatenated**:

  ```java
  em.createNativeQuery("SELECT * FROM users WHERE email = '" + request.getParameter("email") + "'").getResultList();
  ```
* **Safe JPA usage**:

  ```java
  em.createQuery("FROM User u WHERE u.name = :name", User.class)
    .setParameter("name", request.getParameter("name"))
    .getResultList();
  ```

⚠️ Any servlet that builds JPQL/HQL/native SQL from `request` data is at risk.

---

## 🔹 5. Spring JdbcTemplate / Other DB Helpers called inside Servlets

* **Vulnerable** (if servlet passes concatenated SQL):

  ```java
  // servlet calls a helper which does string concat
  jdbcTemplate.query("SELECT * FROM users WHERE role = '" + request.getParameter("role") + "'", rs -> { ... });
  ```
* **Safe**:

  ```java
  jdbcTemplate.query("SELECT * FROM users WHERE role = ?", new Object[]{request.getParameter("role")}, rs -> { ... });
  ```

⚠️ The sink is wherever the SQL is executed — servlet-originated input flows to these helpers.

---

## 🔹 6. MyBatis / XML mappers invoked from Servlets

* **Vulnerable mapper usage (string substitution):**

  ```xml
  <select id="find" resultType="User">
    SELECT * FROM users WHERE name = ${name}
  </select>
  ```

  *Called from servlet with a map containing `name` built from `request.getParameter`.*

* **Safe mapper usage:**

  ```xml
  <select id="find" resultType="User">
    SELECT * FROM users WHERE name = #{name}
  </select>
  ```

⚠️ `${}` expands raw text — dangerous when bound to servlet input.

---

## 🔹 7. Other servlet-sourced inputs that can reach SQL sinks

* `request.getParameter(...)` (query/form)
* `request.getPathInfo()` / `request.getRequestURI()` (path segments)
* `request.getHeader(...)` (headers)
* `request.getCookies()` (cookie values)
* `request.getParts()` (multipart file names / form fields)
* `session.getAttribute(...)` (if session data was set from user input)

⚠️ Treat **all** request-derived strings as untrusted until validated or parameterized.

---

## ✅ Safe Patterns (recommended in Servlets)

* **PreparedStatement parameters**:

  ```java
  PreparedStatement ps = conn.prepareStatement("SELECT * FROM users WHERE id = ?");
  ps.setInt(1, Integer.parseInt(request.getParameter("id")));
  ```
* **Use ORM parameter binding**:

  ```java
  em.createQuery("FROM User u WHERE u.email = :email", User.class)
    .setParameter("email", request.getParameter("email"))
    .getResultList();
  ```
* **Use helper APIs with parameter placeholders** (JdbcTemplate, MyBatis `#{}`).
* **Validate & canonicalize input** before using as a query parameter (type checks, allowlists).
* **Avoid dynamic SQL for identifiers** — if you must build column/table names, validate against an allowlist.
* **Use least-privilege DB account** and limit returned columns where possible.
* **Log and monitor** suspicious patterns (multiple quotes, SQL keywords, unusual length).

---

## 🔹 8. Examples of risky dynamic SQL commonly seen in Servlets

* Building ORDER BY / LIMIT from request without validation:

  ```java
  String sort = request.getParameter("sort"); // "name; DROP TABLE users; --"
  stmt.executeQuery("SELECT * FROM users ORDER BY " + sort);
  ```
* Passing raw JSON fields into native queries:

  ```java
  String payload = request.getReader().lines().collect(Collectors.joining());
  stmt.execute("INSERT INTO audit (data) VALUES ('" + payload + "')");
  ```

⚠️ Dynamic statements that include control tokens (ORDER BY, LIMIT, column names, whole query fragments) must be strictly validated or avoided.

---

## 🔹 9. Quick checklist for reviewing Servlets for SQLi

* Does the servlet concatenate any `request.*` value into SQL? → **fix**
* Are PreparedStatements used correctly with `setX()`? → **good**
* Are ORM/native queries parameterized? → **good**
* Are `${}` style substitutions used in MyBatis? → **danger**
* Are path/header/cookie values validated before DB use? → **validate**
* Is dynamic SQL (identifiers/fragments) built from untrusted input? → **disallow or allowlist**

---

## 🔹 10. Short remediation snippets

* **Replace concatenation:**

  ```java
  // bad
  stmt.executeQuery("SELECT * FROM users WHERE username = '" + request.getParameter("u") + "'");
  // good
  PreparedStatement ps = conn.prepareStatement("SELECT * FROM users WHERE username = ?");
  ps.setString(1, request.getParameter("u"));
  ```
* **Allowlist dynamic identifiers:**

  ```java
  String col = request.getParameter("col");
  if (!List.of("name","created_at","email").contains(col)) throw new IllegalArgumentException();
  String sql = "SELECT * FROM users ORDER BY " + col;
  ```
