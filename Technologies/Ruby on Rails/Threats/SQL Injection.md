# 🔴 **Ruby on Rails – SQL Injection (SQLi) Sinks**

---

## 🔹 1. Raw SQL Execution

* `ActiveRecord::Base.connection.execute("SELECT * FROM users WHERE id = #{userInput}")`
* `connection.exec_query("... #{userInput} ...")`
* `Model.find_by_sql("SELECT * FROM users WHERE name = '#{userInput}'")`

⚠️ Direct string interpolation = **dangerous**.

---

## 🔹 2. ActiveRecord Finder Methods (Interpolated)

* `User.where("name = '#{userInput}'")`
* `User.find_by("email = '#{userInput}'")`
* `User.order("id = #{userInput}")`
* `User.select("id, #{userInput}")`

⚠️ Any **SQL fragment** with `#{}` interpolation = **SQLi risk**.

---

## 🔹 3. Dynamic Scopes & Joins

* `scope :search, ->(q) { where("title LIKE '%#{q}%'") }`
* `User.joins("INNER JOIN #{userInput} ON users.id = ...")`

⚠️ Dangerous if untrusted input is injected into **JOINs / LIKE queries**.

---

## 🔹 4. AREL / Manual Query Building

* `User.where(User.arel_table[:name].eq(userInput).to_sql)`
* Custom string concatenation inside AREL.

⚠️ Usually safe, but becomes vulnerable if devs **convert AREL to raw SQL with interpolation**.

---

## 🔹 5. Red Flags in Code Reviews

* `"SELECT ... #{params[:id]}"`
* `where("... #{params[:query]}")`
* `order(params[:sort])`
* `joins(params[:join_clause])`

---

## ✅ Safe Patterns

* **Bind parameters** (sanitized):

  ```ruby
  User.where("name = ?", userInput)
  ```
* **Hash conditions** (auto-sanitized):

  ```ruby
  User.where(email: params[:email])
  ```
* **Scopes with safe placeholders**:

  ```ruby
  scope :by_status, ->(status) { where(status: status) }
  ```
