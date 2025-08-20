## 1. 🗂️ **Top-Level Project Folder (e.g., `src/`)**

* Created with: `django-admin startproject project_name`

* Contains:

  * **`manage.py`** → Command-line utility for running commands (`runserver`, `migrate`, etc.)

  * **Project Package Folder** (same name as project, e.g., `project_name/`)

---

## 2. 📦 **Project Package (`project_name/`)**

* **`__init__.py`** → Marks folder as a Python package.

* **`settings.py`** → Global project settings (DB, apps, middleware, static, templates).

* **`urls.py`** → Root URL dispatcher → includes app URLs.

* **`asgi.py`** → Entry point for ASGI servers (async, WebSockets).

* **`wsgi.py`** → Entry point for WSGI servers (traditional deployments).

---

## 3. 🛠️ **Apps (`app_name/`)**

Each app handles a specific feature (e.g., blog, user, core).

**App Files:**

* **`__init__.py`** → package marker.

* **`admin.py`** → Register & customize models in admin site.

* **`apps.py`** → Config class for the app.

* **`models.py`** → ORM classes (database tables).

* **`views.py`** → Request handling logic (functions/classes).

* **`urls.py`** → App-level URL routes (usually created manually).

* **`forms.py`** → (Optional) Django forms for handling input.

* **`tests.py`** → Unit tests for the app.

* **`migrations/`** → Auto-generated files for DB schema changes.

**Extra Common Files in Larger Projects:**

* **`serializers.py`** → (DRF) Convert models ↔ JSON.

* **`services.py`** → Business logic (keep views thin).

* **`selectors.py`** → Query logic (read-only database operations).

* **`permissions.py`** → Custom DRF permissions / access control.

* **`utils.py`** → Helper functions & utilities.

---

## 4. 🖼️ **Templates**

* HTML files for rendering views.

* Usually per-app under `templates/app_name/`.

* Configured in `settings.py` under `TEMPLATES['DIRS']`.

---

## 5. 🎨 **Static Files**

* CSS, JS, Images.

* Usually per-app under `static/app_name/`.

* Configured in `settings.py` under `STATICFILES_DIRS`.

---

## 6. 📁 **Media**

* Stores user-uploaded files.

* Configured in `settings.py` with `MEDIA_URL` and `MEDIA_ROOT`.

---

## 7. 📋 **Other Common Files**

* **`requirements.txt`** → List of dependencies.

* **`venv/`** → Virtual environment (often outside project).

* **`.env`** → Environment variables (optional).

---

## 8. 🌳 **Project Tree**

```
project_name/                
│
├── manage.py                # Django command-line tool (runserver, migrate, etc.)
│
├── project_name/            # Main project package (global settings & config)
│   ├── __init__.py          # Marks as a Python package
│   ├── settings.py          # Global settings: DB, apps, middleware, templates, static
│   ├── urls.py              # Root URL dispatcher → routes to app urls
│   ├── asgi.py              # ASGI entry point (async support, WebSockets)
│   └── wsgi.py              # WSGI entry point (deployment on servers)
│
├── app_name/                # Example Django app (feature module)
│   ├── __init__.py          # Marks as a Python package
│   ├── admin.py             # Django admin panel configuration
│   ├── apps.py              # App configuration class
│   ├── models.py            # ORM models (database tables)
│   ├── views.py             # Request/response logic (controllers)
│   ├── urls.py              # App-level URL routes
│   ├── forms.py             # (Optional) Django forms (input validation)
│   ├── serializers.py       # (DRF) Convert models ↔ JSON / API validation
│   ├── services.py          # Business logic (keep views thin)
│   ├── selectors.py         # Query/read logic (DB queries separated from views)
│   ├── permissions.py       # (DRF) Custom permissions & access control
│   ├── utils.py             # Helper functions / utilities
│   ├── tests.py             # Unit tests for the app
│   └── migrations/          # DB schema changes (auto-generated)
│       └── __init__.py
│
├── templates/               # HTML templates for rendering views
│   └── app_name/
│       └── example.html     # Example template
│
├── static/                  # Static files (CSS, JS, images)
│   └── app_name/
│       └── style.css        # Example stylesheet
│
├── media/                   # (Optional) User-uploaded files (images, docs, etc.)
│
├── requirements.txt         # List of project dependencies
└── venv/                    # (Optional) Virtual environment for dependencies
```

---

## 9. 🔄 **How It All Connects (Flow)**

```
User → enters URL → Django checks project/urls.py
      ↓
project/urls.py → routes to app_name/urls.py
      ↓
app_name/urls.py → calls a view function/class
      ↓
views.py → (may call services.py for business logic)
      ↓
services.py → may call selectors.py for DB queries
      ↓
views.py → (may query models.py directly too)
      ↓
models.py → ORM ↔ database
      ↓
serializers.py → (if API) converts data ↔ JSON
      ↓
templates/ → (if web) renders HTML
      ↓
Response → sent back to User
```
