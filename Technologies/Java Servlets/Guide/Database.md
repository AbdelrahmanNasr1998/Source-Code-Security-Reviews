## 🔹 1. Location

* In **pure Servlets/JSP**, there is no automatic datasource configuration.
* Database connection is done manually via **JDBC** inside utility classes or DAOs.
* DB settings can be stored in:

  * `WEB-INF/web.xml` (as `<context-param>`)
  * `db.properties` file (inside `src/main/resources` or `WEB-INF/classes`)

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

`db.properties`

```properties
jdbc.url=jdbc:sqlite:database.sqlite
jdbc.driver=org.sqlite.JDBC
jdbc.username=
jdbc.password=
```

DB Utility Example:

```java
public class DBUtil {
    private static final String URL = "jdbc:sqlite:database.sqlite";

    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(URL);
    }
}
```

---

## 🔹 3. MySQL / MariaDB Example

Dependency:

```xml
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-j</artifactId>
  <scope>runtime</scope>
</dependency>
```

`db.properties`

```properties
jdbc.url=jdbc:mysql://127.0.0.1:3306/mydatabase
jdbc.username=myuser
jdbc.password=mypassword
jdbc.driver=com.mysql.cj.jdbc.Driver
```

DB Utility:

```java
public class DBUtil {
    private static final String URL = "jdbc:mysql://127.0.0.1:3306/mydatabase";
    private static final String USER = "myuser";
    private static final String PASS = "mypassword";

    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(URL, USER, PASS);
    }
}
```

---

## 🔹 4. PostgreSQL Example

Dependency:

```xml
<dependency>
  <groupId>org.postgresql</groupId>
  <artifactId>postgresql</artifactId>
  <scope>runtime</scope>
</dependency>
```

`db.properties`

```properties
jdbc.url=jdbc:postgresql://127.0.0.1:5432/mydatabase
jdbc.username=myuser
jdbc.password=mypassword
jdbc.driver=org.postgresql.Driver
```

---

## 🔹 5. SQL Server Example

Dependency:

```xml
<dependency>
  <groupId>com.microsoft.sqlserver</groupId>
  <artifactId>mssql-jdbc</artifactId>
  <scope>runtime</scope>
</dependency>
```

`db.properties`

```properties
jdbc.url=jdbc:sqlserver://127.0.0.1:1433;databaseName=mydatabase
jdbc.username=myuser
jdbc.password=mypassword
jdbc.driver=com.microsoft.sqlserver.jdbc.SQLServerDriver
```

---

## 🔹 6. Oracle Example

Dependency:

```xml
<dependency>
  <groupId>com.oracle.database.jdbc</groupId>
  <artifactId>ojdbc11</artifactId>
  <version>23.3.0.23.09</version>
</dependency>
```

`db.properties`

```properties
jdbc.url=jdbc:oracle:thin:@127.0.0.1:1521:xe
jdbc.username=myuser
jdbc.password=mypassword
jdbc.driver=oracle.jdbc.OracleDriver
```

---

## 🔹 7. Remote DB with SSL Example (MySQL)

`db.properties`

```properties
jdbc.url=jdbc:mysql://remotehost:3306/mydatabase?useSSL=true&requireSSL=true&verifyServerCertificate=true&serverSslCert=/etc/ssl/certs/ca-cert.pem
jdbc.username=myuser
jdbc.password=mypassword
jdbc.driver=com.mysql.cj.jdbc.Driver
```

---

## 🔹 8. Notes

* In Servlets/JSP, always **load the JDBC driver** before use:

  ```java
  Class.forName("com.mysql.cj.jdbc.Driver");
  ```

  (newer JDBC versions may load automatically).
* Store credentials in `db.properties` or environment variables instead of hardcoding.
* Use **Connection Pooling** (e.g., Apache DBCP, HikariCP) for production apps.
* Close connections, statements, and result sets to avoid memory leaks.
* Use **PreparedStatement** to prevent SQL injection.
