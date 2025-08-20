# 🐘 **PHP & Laravel – User Input Sources**

---

## 🔹 1. Query String (GET parameters)

**Native PHP:**

```php
// Example: GET /search.php?name=admin
$name = $_GET['name'];
```

**Laravel:**

```php
// Example: GET /search?name=admin
$name = $request->query('name');
// Or
$name = $request->input('name');
```

---

## 🔹 2. Route Parameters (Path Variables)

**Native PHP:**

```php
// Example: /user.php?id=42
$id = $_GET['id'];  // (no native route binding)
```

**Laravel:**

```php
Route::get('/user/{id}', function ($id) {
    return "User ID = " . $id;
});
```

---

## 🔹 3. Form Data (POST parameters)

**Native PHP:**

```php
$username = $_POST['username'];
$password = $_POST['password'];
```

**Laravel:**

```php
$username = $request->input('username');
$password = $request->post('password');
```

---

## 🔹 4. JSON Body

**Native PHP:**

```php
$raw = file_get_contents("php://input");
$data = json_decode($raw, true);
$email = $data['email'] ?? null;
```

**Laravel:**

```php
$data = $request->json()->all();
$email = $request->json('email');
```

---

## 🔹 5. File Uploads

**Native PHP (Single File):**

```php
$fileName = $_FILES['avatar']['name'];
$tmpPath  = $_FILES['avatar']['tmp_name'];
```

**Native PHP (Multiple Files):**

```php
foreach ($_FILES['photos']['name'] as $i => $name) {
    $tmpPath = $_FILES['photos']['tmp_name'][$i];
}
```

**Laravel (Single File):**

```php
$file = $request->file('avatar');
$name = $file->getClientOriginalName();
```

**Laravel (Multiple Files):**

```php
foreach ($request->file('photos') as $file) {
    $name = $file->getClientOriginalName();
}
```

---

## 🔹 6. Headers

**Native PHP:**

```php
$ua = $_SERVER['HTTP_USER_AGENT'];
$ip = $_SERVER['HTTP_X_FORWARDED_FOR'];
```

**Laravel:**

```php
$ua = $request->header('User-Agent');
$ip = $request->header('X-Forwarded-For');
```

---

## 🔹 7. Cookies

**Native PHP:**

```php
$session_id = $_COOKIE['session_id'];
```

**Laravel:**

```php
$sessionId = $request->cookie('session_id');
```

---

## 🔹 8. Session Data

**Native PHP:**

```php
session_start();
$user = $_SESSION['user'];
```

**Laravel:**

```php
$user = $request->session()->get('user');
```

---

## 🔹 9. Raw Request Body

**Native PHP:**

```php
$raw = file_get_contents("php://input");
```

**Laravel:**

```php
$raw = $request->getContent();
```

---

## 🔹 10. All Inputs at Once

**Native PHP:**

```php
$data = $_REQUEST; // (GET + POST + COOKIE) ⚠️ unsafe
```

**Laravel:**

```php
$all = $request->all();
```

---

# ✅ **Security Review Checklist (PHP / Laravel)**

* **Trace**: `$_GET`, `$_POST`, `$_COOKIE`, `$_FILES`, `$request->input()`, `$request->file()`, `$request->json()`, `$request->header()`
* **Dangerous sinks**:

  * `eval()`, `exec()`, `system()`, `shell_exec()`
  * `DB::raw()` / unescaped SQL queries → **SQLi**
  * `Blade {!! $var !!}` → **XSS**
  * `unserialize()` → **Object Injection**
* **Special risk in Laravel**: Mass Assignment (`$request->all()` → directly into model).
