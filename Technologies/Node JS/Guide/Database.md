# 📒 Node.js – Database (SQL) Configuration

---

## 🔹 1. Location

* In Node.js, database config usually goes in:

  * `config/db.js` (custom) or
  * environment variables (`.env`).
* Libraries commonly used:

  * **mysql2** or **pg** (Postgres)
  * **mssql** (SQL Server)
  * **oracledb** (Oracle)
  * OR **ORMs** like Sequelize, Prisma, TypeORM.

---

## 🔹 2. SQLite Example

Install driver:

```bash
npm install sqlite3
```

`config/db.js`

```js
const sqlite3 = require('sqlite3').verbose();

const db = new sqlite3.Database('./database.sqlite', (err) => {
  if (err) console.error('Error opening SQLite DB', err);
  else console.log('Connected to SQLite DB');
});

module.exports = db;
```

---

## 🔹 3. MySQL / MariaDB Example

Install driver:

```bash
npm install mysql2
```

`.env`

```env
DB_HOST=127.0.0.1
DB_PORT=3306
DB_USER=myuser
DB_PASS=mypassword
DB_NAME=mydatabase
```

`config/db.js`

```js
const mysql = require('mysql2');

const pool = mysql.createPool({
  host: process.env.DB_HOST,
  port: process.env.DB_PORT,
  user: process.env.DB_USER,
  password: process.env.DB_PASS,
  database: process.env.DB_NAME
});

module.exports = pool.promise();
```

---

## 🔹 4. PostgreSQL Example

Install driver:

```bash
npm install pg
```

`.env`

```env
DB_HOST=127.0.0.1
DB_PORT=5432
DB_USER=myuser
DB_PASS=mypassword
DB_NAME=mydatabase
```

`config/db.js`

```js
const { Pool } = require('pg');

const pool = new Pool({
  host: process.env.DB_HOST,
  port: process.env.DB_PORT,
  user: process.env.DB_USER,
  password: process.env.DB_PASS,
  database: process.env.DB_NAME
});

module.exports = pool;
```

---

## 🔹 5. SQL Server Example

Install driver:

```bash
npm install mssql
```

`.env`

```env
DB_HOST=127.0.0.1
DB_PORT=1433
DB_USER=myuser
DB_PASS=mypassword
DB_NAME=mydatabase
```

`config/db.js`

```js
const sql = require('mssql');

const config = {
  user: process.env.DB_USER,
  password: process.env.DB_PASS,
  server: process.env.DB_HOST,
  port: parseInt(process.env.DB_PORT, 10),
  database: process.env.DB_NAME,
  options: { encrypt: true, trustServerCertificate: true }
};

sql.connect(config).then(() => console.log('Connected to SQL Server'))
  .catch(err => console.error('SQL Server connection error', err));

module.exports = sql;
```

---

## 🔹 6. Oracle Example

Install driver:

```bash
npm install oracledb
```

`.env`

```env
DB_USER=myuser
DB_PASS=mypassword
DB_CONNECT_STRING=127.0.0.1:1521/xe
```

`config/db.js`

```js
const oracledb = require('oracledb');

async function init() {
  try {
    const connection = await oracledb.getConnection({
      user: process.env.DB_USER,
      password: process.env.DB_PASS,
      connectString: process.env.DB_CONNECT_STRING
    });
    console.log('Connected to Oracle DB');
    return connection;
  } catch (err) {
    console.error('Oracle connection error', err);
  }
}

module.exports = init();
```

---

## 🔹 7. Remote DB with SSL Example (MySQL)

`.env`

```env
DB_HOST=remotehost
DB_PORT=3306
DB_USER=myuser
DB_PASS=mypassword
DB_NAME=mydatabase
DB_SSL_CA=/etc/ssl/certs/ca-cert.pem
```

`config/db.js`

```js
const fs = require('fs');
const mysql = require('mysql2');

const pool = mysql.createPool({
  host: process.env.DB_HOST,
  port: process.env.DB_PORT,
  user: process.env.DB_USER,
  password: process.env.DB_PASS,
  database: process.env.DB_NAME,
  ssl: {
    ca: fs.readFileSync(process.env.DB_SSL_CA)
  }
});

module.exports = pool.promise();
```

---

## 🔹 8. Notes

* Use **dotenv** (`npm install dotenv`) to load `.env`:

  ```js
  require('dotenv').config();
  ```
* For production, prefer **connection pools**.
* ORMs (Sequelize, Prisma, TypeORM) simplify cross-database usage.
* Always secure `.env` and avoid committing it to Git.

---
