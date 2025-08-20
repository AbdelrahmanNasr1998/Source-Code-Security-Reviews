## 1. 🗂️ **Top-Level Project Folder**

Created when you start a Flask app (manually or with `flask startproject`):

Contains:

* **`app/`** → Main application package.
* **`venv/`** → Python virtual environment (optional but recommended).
* **`requirements.txt`** → Project dependencies.
* **`run.py`** or **`app.py`** → Entry point for the app.

---

## 2. 📁 **Folders & Responsibilities**

### 🕹️ `app/`

* Core application code. Subfolders:

  * **`routes/`** or `views.py` → Handle HTTP requests.
  * **`models.py`** → Database models (SQLAlchemy).
  * **`services/`** *(optional)* → Business logic layer.
  * **`forms.py`** → WTForms for form validation.
  * **`templates/`** → Jinja2 HTML templates.
  * **`static/`** → CSS, JS, images.
  * **`utils/`** → Helper functions.
  * **`__init__.py`** → Creates Flask app and initializes extensions.

---

### 🗄️ `instance/` *(optional)*

* Holds instance-specific configuration (e.g., `config.py`).

---

### 📝 `config.py`

* Central configuration (DB URI, secret key, debug, etc.).

---

### 🧪 `tests/`

* Unit & integration tests (pytest or unittest).

---

## 3. 🌳 **Project Tree (with Comments)**

```
project-name/
│
├── run.py / app.py           # Entry point (creates & runs Flask app)
├── requirements.txt          # Python dependencies
├── venv/                     # Virtual environment (optional)
│
├── app/                      # Core application package
│   ├── __init__.py           # Initialize app & extensions
│   ├── models.py             # DB models (SQLAlchemy)
│   ├── routes/               # HTTP routes
│   │   └── user_routes.py
│   ├── services/             # (Optional) Business logic layer
│   │   └── user_service.py
│   ├── forms.py              # WTForms for validation
│   ├── templates/            # Jinja2 templates (HTML)
│   │   └── index.html
│   ├── static/               # CSS, JS, images
│   │   ├── style.css
│   │   └── script.js
│   └── utils/                # Helper functions
│       └── helpers.py
│
├── config.py                 # Application configuration
├── instance/                 # (Optional) Instance-specific configs
│   └── config.py
└── tests/                    # Unit & integration tests
    ├── test_routes.py
    └── test_models.py
```

---

## 4. 🔄 **How It All Connects (Flow)**

```
User → sends HTTP request (/users)
      ↓
routes/user_routes.py → matches URL → Controller/Route function
      ↓
Service (user_service.py, optional) → business logic
      ↓
Model (models.py) → interacts with DB via SQLAlchemy
      ↓
Route function → returns response (JSON or template)
      ↓
Template (index.html) → renders HTML (if applicable)
      ↓
Response → sent back to User
```
