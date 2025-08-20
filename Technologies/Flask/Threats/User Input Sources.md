# 🐍 **Python – Flask: User Input Sources**

---

## 🔹 **1. Query Parameters (GET requests)**

* `request.args` → Query string parameters (`?id=123&name=test`)

  ```python
  from flask import request

  name = request.args.get("name")
  ```

---

## 🔹 **2. Form Data (POST requests)**

* `request.form` → Form-encoded POST body (`Content-Type: application/x-www-form-urlencoded`)

  ```python
  username = request.form["username"]
  ```

---

## 🔹 **3. File Uploads**

* `request.files` → Uploaded file(s) (`multipart/form-data`)

  ```python
  file = request.files["avatar"]
  ```

---

## 🔹 **4. JSON Body**

* `request.get_json()` → JSON request body (dict)
* `request.json` → Shortcut for the same

  ```python
  data = request.get_json()
  email = data.get("email")
  ```

---

## 🔹 **5. Raw Request Body**

* `request.data` → Raw request body as bytes
* `request.get_data()` → Same, with optional parsing control

  ```python
  raw = request.data
  ```

---

## 🔹 **6. Path & URL Parameters**

* Defined in route as placeholders:

  ```python
  @app.route("/user/<int:id>")
  def user_detail(id):
      return f"User {id}"
  ```

---

## 🔹 **7. Cookies**

* `request.cookies` → Dictionary of cookies sent by client

  ```python
  session_id = request.cookies.get("session_id")
  ```

---

## 🔹 **8. Headers**

* `request.headers` → HTTP headers

  ```python
  ua = request.headers.get("User-Agent")
  ip = request.headers.get("X-Forwarded-For")
  ```

⚠️ Important: `Host`, `X-Forwarded-For`, and `X-Real-IP` are **common injection vectors**.

---

## 🔹 **9. Sessions (Indirect User Input)**

* Flask session data (signed but **client-stored**) → attacker can tamper if key leaks.

  ```python
  from flask import session

  session["cart"] = ["item1", "item2"]
  ```

---

## 🔹 **10. Other Input Sources (Often Missed)**

* **request.values** → Merged dict of `args` + `form`

  ```python
  val = request.values["param"]
  ```
* **request.stream** → Access raw WSGI input stream (low-level)
* **request.environ** → Full WSGI environment (headers, IPs, etc.)
* **request.files.getlist("file")** → Multiple file uploads

---

# ✅ **Summary (Security Review Checklist)**

When reviewing Flask code, always check:

* `request.args` / `request.form` / `request.files`
* `request.get_json()` / `request.data`
* Path variables (`<id>`, `<string:name>`, etc.)
* `request.cookies`
* `request.headers`
* `session` (client-side, signed cookies)
* `request.values`, `request.stream`, `request.environ`

🚨 Dangerous patterns to flag:

* Passing **any of these inputs** to `eval`, `exec`, `subprocess`, `os.system`, `yaml.load`, or template rendering (`render_template_string`) without sanitization → possible **RCE / injection**.
