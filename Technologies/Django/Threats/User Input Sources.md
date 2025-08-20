# 🐍 **Python – Django: User Input Sources**

---

## 🔹 **1. Query Parameters (GET requests)**

* `request.GET` → Query string parameters (`?id=123&name=test`)

  ```python
  name = request.GET.get("name")
  ```

---

## 🔹 **2. Form Data (POST requests)**

* `request.POST` → Form-encoded POST body (`Content-Type: application/x-www-form-urlencoded`)

  ```python
  username = request.POST["username"]
  ```

---

## 🔹 **3. File Uploads**

* `request.FILES` → Uploaded file(s) (`multipart/form-data`)

  ```python
  file = request.FILES["document"]
  ```

---

## 🔹 **4. Raw Request Body**

* `request.body` → Raw bytes from request body (useful for JSON, XML, custom payloads)

  ```python
  raw = request.body
  ```

---

## 🔹 **5. JSON / DRF Data**

* `request.data` (Django REST Framework) → Automatically parses JSON, form, or multipart requests

  ```python
  data = request.data["email"]
  ```

---

## 🔹 **6. Path & URL Parameters**

* Defined in `urls.py` as dynamic segments:

  ```python
  path("user/<int:id>/", views.user_detail)
  ```

  → Accessible in view as function argument:

  ```python
  def user_detail(request, id):
      return HttpResponse(id)
  ```

---

## 🔹 **7. Cookies**

* `request.COOKIES` → Dictionary of cookies sent by client

  ```python
  sessionid = request.COOKIES.get("sessionid")
  ```

---

## 🔹 **8. Headers / Meta**

* `request.META` → All request headers and server vars

  ```python
  user_agent = request.META.get("HTTP_USER_AGENT")
  ip = request.META.get("REMOTE_ADDR")
  ```

⚠️ Critical: many attacks hide in headers (`X-Forwarded-For`, `Host`, `User-Agent`, etc.).

---

## 🔹 **9. Sessions (Indirect User Input)**

* `request.session` → Can store/retrieve attacker-controlled data if not validated

  ```python
  cart = request.session.get("cart", [])
  ```

---

## 🔹 **10. Other Input Sources (Often Missed)**

* **QueryDict copies** → `request.GET.copy()`, `request.POST.copy()`
* **request.content\_params** (content negotiation, DRF)
* **request.query\_params** (DRF alias for `request.GET`)
* **Streaming data**: `request.stream` (low-level raw body reading)

---

# ✅ **Summary (Checklist for Security Review)**

When reviewing Django code, always check these input sources:

* `request.GET` / `request.POST` / `request.FILES`
* `request.body` / `request.data` (DRF)
* **Path variables** from `urls.py`
* `request.COOKIES`
* `request.META` (headers, IP, host)
* `request.session`
* Any use of **YAML, pickle, eval** on these inputs → 🚨 potential RCE
