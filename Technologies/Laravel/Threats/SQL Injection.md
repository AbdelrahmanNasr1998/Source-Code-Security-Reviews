# 🐘 **Laravel – SQL Injection (SQLi) Sinks**

---

## 🔹 1. Raw SQL Execution

* `DB::select($sql)`
* `DB::insert($sql)`
* `DB::update($sql)`
* `DB::delete($sql)`
* `DB::statement($sql)`

⚠️ Vulnerable if `$sql` is concatenated with user input.

---

## 🔹 2. Query Builder – Unsafe

* `DB::table('users')->whereRaw("name = '$userInput'")->get()`
* `DB::raw("... $userInput ...")`

❌ Any use of `whereRaw()`, `havingRaw()`, `orderByRaw()`, etc. with direct input.

---

## 🔹 3. Eloquent ORM – Unsafe Patterns

* `User::whereRaw("email = '$userInput'")->get()`
* `User::where("email", "=", DB::raw($userInput))->get()`

⚠️ Dangerous when mixing `DB::raw()` with user input.

---

## 🔹 4. Red Flags in Code Reviews

* `"SELECT * FROM users WHERE email = '$userInput'"`
* `"... WHERE id = " . $_GET['id']`
* String interpolation inside queries: `"SELECT ... {$userInput}"`

---

## ✅ Safe Patterns

* Use **parameter binding**:

  ```php
  DB::select('SELECT * FROM users WHERE email = ?', [$userInput]);
  ```
* Use **query builder bindings**:

  ```php
  DB::table('users')->where('email', $userInput)->get();
  ```
* Use **Eloquent safe queries**:

  ```php
  User::where('email', $userInput)->first();
  ```
