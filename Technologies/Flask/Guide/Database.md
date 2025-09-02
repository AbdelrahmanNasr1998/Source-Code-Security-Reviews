In **Flask**, the SQL database configuration is usually stored in the **`config.py`** file (or sometimes directly in `app.py` for small projects).
You configure it using the **`SQLALCHEMY_DATABASE_URI`** variable if you’re using **Flask-SQLAlchemy**.

---

# 📒 Flask – Database (SQL) Configuration

---

## 🔹 1. Location

* File: `config.py` (or inside `app.py`)
* Section: `SQLALCHEMY_DATABASE_URI`

---

## 🔹 2. Default Example (SQLite – Flask’s simple option)

```python
# config.py
SQLALCHEMY_DATABASE_URI = 'sqlite:///mydatabase.db'
SQLALCHEMY_TRACK_MODIFICATIONS = False
```

---

## 🔹 3. PostgreSQL Example

```python
# config.py
SQLALCHEMY_DATABASE_URI = 'postgresql://myuser:mypassword@localhost:5432/mydatabase'
```

---

## 🔹 4. MySQL Example

```python
# config.py
SQLALCHEMY_DATABASE_URI = 'mysql+pymysql://myuser:mypassword@127.0.0.1:3306/mydatabase'
```

---

## 🔹 5. MariaDB Example

```python
# config.py
SQLALCHEMY_DATABASE_URI = 'mysql+pymysql://myuser:mypassword@localhost:3306/mydatabase'
```

---

## 🔹 6. SQL Server (using `pyodbc`)

```python
# config.py
SQLALCHEMY_DATABASE_URI = 'mssql+pyodbc://myuser:mypassword@localhost:1433/mydatabase?driver=ODBC+Driver+17+for+SQL+Server'
```

---

## 🔹 7. Remote DB with SSL Example

```python
# config.py
SQLALCHEMY_DATABASE_URI = 'postgresql://myuser:mypassword@remotehost:5432/mydatabase'

# Secure SSL connection (extra params)
SQLALCHEMY_ENGINE_OPTIONS = {
    "connect_args": {
        "sslmode": "require",
        "sslcert": "/path/to/client-cert.pem",
        "sslkey": "/path/to/client-key.pem",
        "sslrootcert": "/path/to/ca-cert.pem"
    }
}
```

---

## 🔹 8. Notes

* The **SQLAlchemy URI** format is:

  ```
  dialect+driver://username:password@host:port/database
  ```

  Examples:

  * SQLite → `sqlite:///file.db`
  * PostgreSQL → `postgresql://user:pass@host:port/db`
  * MySQL/MariaDB → `mysql+pymysql://user:pass@host/db`
  * SQL Server → `mssql+pyodbc://user:pass@host/db?driver=...`

* You must install the proper Python package:

  * PostgreSQL → `psycopg2-binary`
  * MySQL/MariaDB → `pymysql`
  * SQL Server → `pyodbc`

* Initialize in `app.py`:

  ```python
  from flask import Flask
  from flask_sqlalchemy import SQLAlchemy
  from config import SQLALCHEMY_DATABASE_URI, SQLALCHEMY_TRACK_MODIFICATIONS

  app = Flask(__name__)
  app.config.from_object('config')
  db = SQLAlchemy(app)
  ```

* Create tables:

  ```bash
  flask shell
  >>> from app import db
  >>> db.create_all()
  ```
