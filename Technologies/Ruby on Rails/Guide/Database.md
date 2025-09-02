# 📒 Ruby on Rails – Database (SQL) Configuration

---

## 🔹 1. Location

* Rails database settings are stored in:

  * `config/database.yml`

* Rails uses **ActiveRecord** as the ORM.

* Different environments (`development`, `test`, `production`) each have their own settings.

---

## 🔹 2. SQLite Example (default in Rails)

`config/database.yml`

```yaml
default: &default
  adapter: sqlite3
  pool: 5
  timeout: 5000

development:
  <<: *default
  database: db/development.sqlite3

test:
  <<: *default
  database: db/test.sqlite3

production:
  <<: *default
  database: db/production.sqlite3
```

---

## 🔹 3. MySQL / MariaDB Example

Add gem in `Gemfile`:

```ruby
gem 'mysql2'
```

`config/database.yml`

```yaml
default: &default
  adapter: mysql2
  encoding: utf8
  pool: 5
  username: myuser
  password: mypassword
  host: 127.0.0.1

development:
  <<: *default
  database: mydatabase_dev

test:
  <<: *default
  database: mydatabase_test

production:
  <<: *default
  database: mydatabase_prod
  username: <%= ENV['DB_USERNAME'] %>
  password: <%= ENV['DB_PASSWORD'] %>
```

---

## 🔹 4. PostgreSQL Example

Add gem:

```ruby
gem 'pg'
```

`config/database.yml`

```yaml
default: &default
  adapter: postgresql
  encoding: unicode
  pool: 5
  username: myuser
  password: mypassword
  host: 127.0.0.1

development:
  <<: *default
  database: mydatabase_dev

test:
  <<: *default
  database: mydatabase_test

production:
  <<: *default
  database: mydatabase_prod
  username: <%= ENV['DB_USERNAME'] %>
  password: <%= ENV['DB_PASSWORD'] %>
```

---

## 🔹 5. SQL Server Example

Add gem:

```ruby
gem 'tiny_tds'
gem 'activerecord-sqlserver-adapter'
```

`config/database.yml`

```yaml
default: &default
  adapter: sqlserver
  host: 127.0.0.1
  port: 1433
  username: myuser
  password: mypassword

development:
  <<: *default
  database: mydatabase_dev
```

---

## 🔹 6. Oracle Example

Add gem:

```ruby
gem 'ruby-oci8'
gem 'activerecord-oracle_enhanced-adapter'
```

`config/database.yml`

```yaml
default: &default
  adapter: oracle_enhanced
  username: myuser
  password: mypassword
  host: 127.0.0.1
  port: 1521
  database: XE

development:
  <<: *default
```

---

## 🔹 7. Remote DB with SSL Example (PostgreSQL)

`config/database.yml`

```yaml
production:
  adapter: postgresql
  encoding: unicode
  pool: 5
  host: remotehost
  port: 5432
  database: mydatabase_prod
  username: myuser
  password: mypassword
  sslmode: verify-full
  sslrootcert: /etc/ssl/certs/ca-cert.pem
```

---

## 🔹 8. Notes

* Rails environments: `development`, `test`, `production`.
* Use **environment variables** (`ENV['DB_PASSWORD']`) for security.
* Use SSL/TLS for production databases.
* Rails migrations manage schema automatically (`rails db:migrate`).
