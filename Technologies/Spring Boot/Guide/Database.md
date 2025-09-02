# 📒 Spring Boot – Database (SQL) Configuration

---

## 🔹 1. Location

* Database settings are stored in:

  * `src/main/resources/application.properties`
  * OR `src/main/resources/application.yml`
* Spring Boot automatically configures the datasource based on these files.

---

## 🔹 2. SQLite Example

Add dependency (in `pom.xml`):

```xml
<dependency>
  <groupId>org.xerial</groupId>
  <artifactId>sqlite-jdbc</artifactId>
  <version>3.45.1.0</version>
</dependency>
```

`application.properties`

```properties
spring.datasource.url=jdbc:sqlite:database.sqlite
spring.datasource.driver-class-name=org.sqlite.JDBC
spring.jpa.database-platform=org.hibernate.dialect.SQLiteDialect
```

---

## 🔹 3. MySQL / MariaDB Example

Add dependency:

```xml
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-j</artifactId>
  <scope>runtime</scope>
</dependency>
```

`application.properties`

```properties
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/mydatabase
spring.datasource.username=myuser
spring.datasource.password=mypassword
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.hibernate.ddl-auto=update
```

---

## 🔹 4. PostgreSQL Example

Add dependency:

```xml
<dependency>
  <groupId>org.postgresql</groupId>
  <artifactId>postgresql</artifactId>
  <scope>runtime</scope>
</dependency>
```

`application.properties`

```properties
spring.datasource.url=jdbc:postgresql://127.0.0.1:5432/mydatabase
spring.datasource.username=myuser
spring.datasource.password=mypassword
spring.jpa.hibernate.ddl-auto=update
```

---

## 🔹 5. SQL Server Example

Add dependency:

```xml
<dependency>
  <groupId>com.microsoft.sqlserver</groupId>
  <artifactId>mssql-jdbc</artifactId>
  <scope>runtime</scope>
</dependency>
```

`application.properties`

```properties
spring.datasource.url=jdbc:sqlserver://127.0.0.1:1433;databaseName=mydatabase
spring.datasource.username=myuser
spring.datasource.password=mypassword
spring.jpa.hibernate.ddl-auto=update
```

---

## 🔹 6. Oracle Example

Add dependency:

```xml
<dependency>
  <groupId>com.oracle.database.jdbc</groupId>
  <artifactId>ojdbc11</artifactId>
  <version>23.3.0.23.09</version>
</dependency>
```

`application.properties`

```properties
spring.datasource.url=jdbc:oracle:thin:@127.0.0.1:1521:xe
spring.datasource.username=myuser
spring.datasource.password=mypassword
spring.jpa.hibernate.ddl-auto=update
```

---

## 🔹 7. Remote DB with SSL Example (MySQL)

`application.properties`

```properties
spring.datasource.url=jdbc:mysql://remotehost:3306/mydatabase?useSSL=true&requireSSL=true&verifyServerCertificate=true&serverSslCert=/etc/ssl/certs/ca-cert.pem
spring.datasource.username=myuser
spring.datasource.password=mypassword
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

---

## 🔹 8. Notes

* Spring Boot supports multiple DBs via **profiles** (e.g., `application-dev.properties`, `application-prod.properties`).
* Use `spring.jpa.hibernate.ddl-auto` (`validate`, `update`, `create`, `create-drop`) carefully in production.
* Use SSL/TLS for production databases.
* Use environment variables or `application.yml` for secrets instead of hardcoding.

---
