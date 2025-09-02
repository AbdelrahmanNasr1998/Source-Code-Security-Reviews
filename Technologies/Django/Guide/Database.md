In **Django**, the SQL database configuration is stored in the **`settings.py`** file of your project.
This file is usually located in the project folder (the one that has the same name as your project, next to `urls.py` and `wsgi.py`).

---

# 📒 Django – Database (SQL) Configuration

---

## 🔹 1. Location

* File: `project_name/settings.py`
* Section: `DATABASES`

---

## 🔹 2. Default Example (SQLite – Django’s default)

```python
# settings.py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',   # SQLite engine
        'NAME': BASE_DIR / 'db.sqlite3',          # File path
    }
}
```

---

## 🔹 3. PostgreSQL Example

```python
# settings.py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',  # PostgreSQL backend
        'NAME': 'mydatabase',                       # Database name
        'USER': 'myuser',                           # Username
        'PASSWORD': 'mypassword',                   # Password
        'HOST': 'localhost',                        # Database host
        'PORT': '5432',                             # Default PostgreSQL port
    }
}
```

---

## 🔹 4. MySQL Example

```python
# settings.py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',       # MySQL backend
        'NAME': 'mydatabase',                       # Database name
        'USER': 'myuser',                           # Username
        'PASSWORD': 'mypassword',                   # Password
        'HOST': '127.0.0.1',                        # Database host
        'PORT': '3306',                             # Default MySQL port
    }
}
```

---

## 🔹 5. MariaDB Example

```python
# settings.py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',       # Same as MySQL backend
        'NAME': 'mydatabase',
        'USER': 'myuser',
        'PASSWORD': 'mypassword',
        'HOST': 'localhost',
        'PORT': '3306',
    }
}
```

---

## 🔹 6. SQL Server (using `mssql-django` package)

```python
# settings.py
DATABASES = {
    'default': {
        'ENGINE': 'mssql',
        'NAME': 'mydatabase',
        'USER': 'myuser',
        'PASSWORD': 'mypassword',
        'HOST': 'localhost',
        'PORT': '1433',                  # Default SQL Server port
        'OPTIONS': {
            'driver': 'ODBC Driver 17 for SQL Server',
        },
    }
}
```

---

## 🔹 7. Remote DB with SSL (Example: PostgreSQL)

```python
# settings.py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'mydatabase',                  # Database name
        'USER': 'myuser',                      # DB username
        'PASSWORD': 'mypassword',              # DB password
        'HOST': 'db.example.com',              # Remote DB host
        'PORT': '5432',                        # PostgreSQL default port
        'OPTIONS': {
            'sslmode': 'require',              # Enforce SSL connection
            # 'sslrootcert': '/path/to/server-ca.pem',   # (optional) CA cert
            # 'sslcert': '/path/to/client-cert.pem',     # (optional) Client cert
            # 'sslkey': '/path/to/client-key.pem',       # (optional) Client key
        },
    }
}
```

### 🔹 Explanation

* **`HOST`** → Remote hostname or IP (`db.example.com`, `10.0.0.5`, etc.)
* **`OPTIONS`** → SSL-related settings

  * `sslmode: require` → forces encrypted connection
  * `sslrootcert`, `sslcert`, `sslkey` → used if the DB server requires client-side certs

---

## 🔹 8. Notes

* The database **engine** determines which backend Django will use:

  * `django.db.backends.sqlite3`
  * `django.db.backends.postgresql`
  * `django.db.backends.mysql`
  * `mssql` (via extra package)
* You must install the proper Python adapter:

  * PostgreSQL → `psycopg2`
  * MySQL/MariaDB → `mysqlclient` or `PyMySQL`
  * SQL Server → `mssql-django` + ODBC driver
* Run migrations after configuring:

  ```bash
  python manage.py migrate
  ```
