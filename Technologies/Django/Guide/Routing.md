# 🛠️ Django URL Dispatcher Flow

When a request comes in, Django follows this flow:

---

### 1. 🌐 Browser Request

* User enters:
  `http://example.com/blog/1/`
* The request hits Django’s **`urls.py`** files.

---

### 2. 📂 Project `urls.py` (root level)

* The main `urls.py` (inside project folder, e.g., `src/urls.py`) defines the global routes.
* It usually **includes** app-level `urls.py` using `include()`.

```python
from django.urls import path, include

urlpatterns = [
    path("admin/", admin.site.urls),
    path("blog/", include("blog.urls")),  # send all /blog/* requests to blog app
]
```

---

### 3. 📄 App `urls.py` (e.g., blog/urls.py)

* Defines paths for that specific app.

```python
from django.urls import path
from . import views

urlpatterns = [
    path("<int:post_id>/", views.post_detail, name="post_detail"),
]
```

---

### 4. 👨‍💻 View Function (in `views.py`)

* Django calls the matching function when URL pattern matches.

```python
from django.http import HttpResponse

def post_detail(request, post_id):
    return HttpResponse(f"Post ID: {post_id}")
```

---

### 5. 🖼️ Template Rendering (Optional)

* If the view returns an HTML page, it usually calls `render()` to connect to a template.

```python
from django.shortcuts import render

def post_detail(request, post_id):
    return render(request, "blog/post_detail.html", {"id": post_id})
```

---

## 🔄 Flow Summary

`🌐 Browser URL → 📂 Project urls.py → 📄 App urls.py → 👨‍💻 views.py function → 🖼️ (optional) template.html`
