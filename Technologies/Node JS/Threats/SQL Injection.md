# 🟢 **Node.js – SQL Injection (SQLi) Sinks**

---

## 🔹 1. Raw SQL Execution (MySQL / PostgreSQL / SQLite, etc.)

* `connection.query("SELECT * FROM users WHERE id = " + userInput)`
* `pool.query("... " + userInput)`
* `client.query("... ${userInput} ...")` (PostgreSQL)

⚠️ Dangerous when concatenating strings or using template literals.

---

## 🔹 2. ORM – Sequelize (Raw & Unsafe)

* `sequelize.query("SELECT * FROM users WHERE name = '" + userInput + "'")`
* `Model.findAll({ where: sequelize.literal("name = '" + userInput + "'") })`

⚠️ Any use of `sequelize.literal()` with untrusted input.

---

## 🔹 3. ORM – TypeORM

* `createQueryBuilder("user").where("user.name = '" + userInput + "'").getMany()`
* `queryRunner.query("SELECT * FROM users WHERE email = '" + userInput + "'")`

⚠️ Direct concatenation = vulnerable.

---

## 🔹 4. Raw MongoDB Queries (NoSQLi Risk)

* `db.collection("users").find({ "$where": "this.age == " + userInput })`
* Passing user input directly into `$where`, `$regex`, or `eval`-like operators.

⚠️ Can lead to **NoSQL injection → RCE in some cases**.

---

## 🔹 5. Red Flags in Code Reviews

* `"SELECT * FROM ... " + req.query.param`
* `` `SELECT * FROM users WHERE name = ${req.body.name}` ``
* ORM raw methods (`literal`, `query`) used with **req.body / req.query** directly.

---

## ✅ Safe Patterns

* Use **prepared statements / parameterized queries**:

  ```js
  connection.query("SELECT * FROM users WHERE id = ?", [userInput]);
  ```
* Use ORM’s built-in **bind parameters**:

  ```js
  sequelize.query("SELECT * FROM users WHERE id = ?", { replacements: [userInput] });
  ```
* For MongoDB → validate input, avoid `$where`, prefer strict schema validation.
