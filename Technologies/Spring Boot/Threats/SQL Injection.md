# ☕ **Spring Boot – SQL Injection (SQLi) Sinks**

---

## 🔹 1. JDBC (Raw SQL)

* `Statement stmt = conn.createStatement(); stmt.execute("SELECT * FROM users WHERE id = " + userInput);`
* `stmt.executeQuery("SELECT * FROM users WHERE name = '" + userInput + "'");`
* `stmt.executeUpdate("UPDATE users SET role = '" + userInput + "'");`

⚠️ Direct concatenation in `Statement` = **classic SQLi**.

---

## 🔹 2. Spring JdbcTemplate

* `jdbcTemplate.query("SELECT * FROM users WHERE name = '" + userInput + "'");`
* `jdbcTemplate.update("DELETE FROM users WHERE id = " + userInput);`
* `jdbcTemplate.batchUpdate("INSERT INTO logs VALUES ('" + userInput + "')");`

⚠️ Vulnerable when SQL string is built from **unsanitized input**.

---

## 🔹 3. JPA / Hibernate (JPQL & Native Queries)

* `em.createQuery("SELECT u FROM User u WHERE u.name = '" + userInput + "'").getResultList();`
* `em.createNativeQuery("SELECT * FROM users WHERE email = '" + userInput + "'").getResultList();`
* `@Query("SELECT u FROM User u WHERE u.name = :#{#userInput}")` (if `SpEL` is misused with raw interpolation).

⚠️ **JPQL** & **native queries** vulnerable if built via concatenation.

---

## 🔹 4. Spring Data Repositories

* `@Query("SELECT u FROM User u WHERE u.name = ?#{[0]}")` (if input is directly concatenated).
* Repositories with custom queries:

  ```java
  @Query("SELECT u FROM User u WHERE u.email = '" + userInput + "'")
  ```

---

## 🔹 5. MyBatis / Other ORMs

* MyBatis mapper with:

  ```xml
  <select id="findUser" resultType="User">
    SELECT * FROM users WHERE name = ${userInput}
  </select>
  ```

  (`${}` → vulnerable, `#{}` → safe).

⚠️ Look out for **`${}` string substitution**.

---

## ✅ Safe Patterns

* **Prepared Statements / Placeholders**:

  ```java
  PreparedStatement ps = conn.prepareStatement("SELECT * FROM users WHERE id = ?");
  ps.setInt(1, userInput);
  ```

* **JdbcTemplate with parameters**:

  ```java
  jdbcTemplate.query("SELECT * FROM users WHERE id = ?", new Object[]{userInput});
  ```

* **JPA Named Parameters**:

  ```java
  em.createQuery("SELECT u FROM User u WHERE u.email = :email")
    .setParameter("email", userInput);
  ```

* **MyBatis `#{}` binding** (safe).
