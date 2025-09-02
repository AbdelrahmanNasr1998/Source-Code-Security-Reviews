# 📒 Laravel – Database (SQL) Configuration

---

## 🔹 1. Location

* Main file: **`.env`** → holds credentials
* Extra setup: **`config/database.php`** → uses `.env` values

---

## 🔹 2. Default Example (SQLite)

```env
# .env
DB_CONNECTION=sqlite
DB_DATABASE=/absolute/path/to/database.sqlite
```

`config/database.php` will automatically read from `.env`.

---

## 🔹 3. MySQL / MariaDB Example

```env
# .env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=mydatabase
DB_USERNAME=myuser
DB_PASSWORD=mypassword
```

---

## 🔹 4. PostgreSQL Example

```env
# .env
DB_CONNECTION=pgsql
DB_HOST=127.0.0.1
DB_PORT=5432
DB_DATABASE=mydatabase
DB_USERNAME=myuser
DB_PASSWORD=mypassword
```

---

## 🔹 5. SQL Server Example

```env
# .env
DB_CONNECTION=sqlsrv
DB_HOST=127.0.0.1
DB_PORT=1433
DB_DATABASE=mydatabase
DB_USERNAME=myuser
DB_PASSWORD=mypassword
```

---

## 🔹 6. Oracle Example (with package)

Laravel does not support Oracle **out of the box**, but you can use [yajra/laravel-oci8](https://github.com/yajra/laravel-oci8).

```env
# .env
DB_CONNECTION=oracle
DB_HOST=127.0.0.1
DB_PORT=1521
DB_DATABASE=xe
DB_USERNAME=myuser
DB_PASSWORD=mypassword
```

`config/database.php` must include the `oracle` driver if package installed.

---

## 🔹 7. Remote DB with SSL Example (MySQL)

```env
# .env
DB_CONNECTION=mysql
DB_HOST=remotehost
DB_PORT=3306
DB_DATABASE=mydatabase
DB_USERNAME=myuser
DB_PASSWORD=mypassword
MYSQL_ATTR_SSL_CA=/etc/ssl/certs/ca-cert.pem
```

* For PostgreSQL: set `DB_CONNECTION=pgsql` and add `sslmode=require` in **`options`** array in `config/database.php`.
* For SQL Server: add `Encrypt=yes;TrustServerCertificate=false;` in the **ODBC connection string**.

---

## 🔹 8. Notes

* Laravel uses **Eloquent ORM** to connect and manage DB.
* To change DB → update `.env` and run `php artisan config:clear`.
* Migration workflow:

```bash
php artisan make:migration create_users_table
php artisan migrate
```

* Example **database.php** section for MySQL:

```php
'mysql' => [
    'driver' => 'mysql',
    'host' => env('DB_HOST', '127.0.0.1'),
    'port' => env('DB_PORT', '3306'),
    'database' => env('DB_DATABASE', 'forge'),
    'username' => env('DB_USERNAME', 'forge'),
    'password' => env('DB_PASSWORD', ''),
    'unix_socket' => env('DB_SOCKET', ''),
    'charset' => 'utf8mb4',
    'collation' => 'utf8mb4_unicode_ci',
    'prefix' => '',
    'strict' => true,
    'engine' => null,
    'options' => extension_loaded('pdo_mysql') ? array_filter([
        PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
    ]) : [],
],
```
e, Laravel)?
