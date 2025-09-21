## 1. 🗂️ **Top-Level Project Folder**

Created when you run:

```bash
spring init --dependencies=web,data-jpa,mysql demo
```

or via **Spring Initializr**.

Contains:

* **`pom.xml`** (Maven) or **`build.gradle`** (Gradle) → Dependency management.

* **`src/`** → Main source code.

* **`target/`** → Compiled build output (ignored in Git).

---

## 2. 📁 **Folders & Responsibilities**

### 🟢 `src/main/java/com/example/project/`

* **Application.java** → Main entry point (contains `@SpringBootApplication`).

### 🕹️ `controller/`

* Handles incoming HTTP requests.

* Annotated with `@RestController` or `@Controller`.

* Defines endpoints with `@GetMapping`, `@PostMapping`, etc.

### 📦 `model/` (or `entity/`)

* Data objects, often mapped to DB tables using JPA (`@Entity`).

### 🗄️ `repository/`

* Interfaces extending `JpaRepository` or `CrudRepository`.

* Handle DB queries.

### 🔧 `service/`

* Business logic layer.

* Annotated with `@Service`.

### 📤 `dto/` *(Data Transfer Objects, optional)*

* Used to transfer data between layers without exposing entities.

### ⚙️ `config/`

* Configuration classes (e.g., security, CORS, custom beans).

### 🛡️ `exception/`

* Custom exception classes & handlers.

### 🧩 `util/`

* Helper classes (e.g., DateUtils, StringUtils).

---

## 3. 📂 **Other Folders**

### 📝 `src/main/resources/`

* **`application.properties`** or **`application.yml`** → Main configuration (DB, ports, etc.).

* **`static/`** → Static resources (CSS, JS, images).

* **`templates/`** → Thymeleaf or Freemarker HTML templates (if SSR).

* **`data.sql`** → Initial data load script.

* **`schema.sql`** → DB schema script.

### 🧪 `src/test/java/`

* Unit & integration tests.

* Annotated with `@SpringBootTest`.

---

## 4. 🌳 **Project Tree**

```
project-name/
│
├── pom.xml / build.gradle     # Dependency management
├── src/
│   ├── main/
│   │   ├── java/com/example/project/
│   │   │   ├── ProjectApplication.java  # Main entry point (@SpringBootApplication)
│   │   │   │
│   │   │   ├── controller/              # Handle requests
│   │   │   │   └── UserController.java  # Example REST controller
│   │   │   │
│   │   │   ├── model/ (or entity/)      # Data models (JPA entities)
│   │   │   │   └── User.java            # Example entity
│   │   │   │
│   │   │   ├── repository/              # Data access layer (JpaRepository)
│   │   │   │   └── UserRepository.java
│   │   │   │
│   │   │   ├── service/                 # Business logic layer
│   │   │   │   └── UserService.java
│   │   │   │
│   │   │   ├── dto/                     # (Optional) Data Transfer Objects
│   │   │   │   └── UserDto.java
│   │   │   │
│   │   │   ├── config/                  # App configuration (Security, Beans, etc.)
│   │   │   │   └── WebSecurityConfig.java
│   │   │   │
│   │   │   ├── exception/               # Custom exceptions & handlers
│   │   │   │   └── GlobalExceptionHandler.java
│   │   │   │
│   │   │   └── util/                    # Utility/helper classes
│   │   │       └── DateUtils.java
│   │   │
│   │   └── resources/
│   │       ├── application.properties   # Main configuration
│   │       ├── static/                  # Static resources (CSS, JS, images)
│   │       │   └── style.css
│   │       ├── templates/               # Server-side templates (Thymeleaf, etc.)
│   │       │   └── index.html
│   │       ├── data.sql                 # Initial data script
│   │       └── schema.sql               # DB schema script
│   │
│   └── test/java/com/example/project/
│       └── ProjectApplicationTests.java # Unit/integration tests
│
└── target/                              # Compiled build output
```

---

## 5. 🔄 **How It All Connects (Flow)**

```
User → sends HTTP request (/users)
      ↓
Controller (UserController) → handles request
      ↓
Service (UserService) → business logic
      ↓
Repository (UserRepository) → fetch/save data from DB
      ↓
Model/Entity (User) → maps to DB table via JPA
      ↓
DTO (optional) → used to send clean data back
      ↓
Controller → returns JSON (REST API) or View (Thymeleaf)
      ↓
Response → sent back to User
```

---

## 6. 🏗️ **Compiled Output (Build Artifacts)**

When you run:

```bash
mvn clean package
```

or

```bash
gradle build
```

Spring Boot creates a **fat JAR** (also called *Uber JAR*) inside the **`target/`** folder.

### 📂 **target/** (Spring Boot build output)

```
project-name/
│
├── target/
│   ├── classes/                         # Compiled .class files
│   │   └── com/example/project/
│   │       ├── ProjectApplication.class
│   │       ├── controller/UserController.class
│   │       ├── service/UserService.class
│   │       ├── repository/UserRepository.class
│   │       ├── model/User.class
│   │       └── util/DateUtils.class
│   │
│   ├── test-classes/                    # Compiled test classes
│   │   └── com/example/project/
│   │       └── ProjectApplicationTests.class
│   │
│   ├── project-name-0.0.1-SNAPSHOT.jar  # Final fat JAR (deployable & runnable)
│   ├── project-name-0.0.1-SNAPSHOT.jar.original  # Before Spring Boot repackaging
│   └── maven-archiver/                  # Build metadata
│       └── pom.properties
```

---

## 7. 📦 **Inside the Spring Boot JAR**

Spring Boot’s fat JAR is self-contained (you can run it directly with `java -jar project-name.jar`).
If you unzip it, the structure looks like this:

```
project-name-0.0.1-SNAPSHOT.jar
│
├── BOOT-INF/
│   ├── classes/             # Your compiled classes (controllers, services, etc.)
│   │   └── com/example/project/...
│   │
│   ├── lib/                 # All dependencies (Spring Boot, MySQL driver, etc.)
│   │   ├── spring-boot-starter-web-3.1.0.jar
│   │   ├── spring-data-jpa-3.1.0.jar
│   │   ├── mysql-connector-j-8.0.33.jar
│   │   └── ...
│   │
│   └── META-INF/
│       └── MANIFEST.MF      # Metadata (main class, build info)
│
└── org/springframework/boot/loader/  # Spring Boot launcher classes
```

---

## 8. 🧾 **Notes on Spring Boot JARs**

* **`project-name-0.0.1-SNAPSHOT.jar`** → The **fat JAR** (contains everything to run app).
* **`BOOT-INF/classes/`** → Your project’s compiled code (`controller/`, `service/`, etc.).
* **`BOOT-INF/lib/`** → Dependencies (Spring, Hibernate, DB drivers, etc.).
* **`org.springframework.boot.loader`** → The special **Spring Boot launcher** that makes the fat JAR executable.
* **`.jar.original`** → A plain JAR without Spring Boot packaging (generated first, then repackaged).
* Unlike traditional WAR files:

  * Spring Boot embeds Tomcat/Jetty/Undertow inside the JAR.
  * You can run `java -jar project-name.jar` without needing to deploy to an external server.

---

👉 This way, your **source folders (`controller/`, `service/`, `repository/`, etc.)** map directly into **`BOOT-INF/classes/`**, while dependencies go into **`BOOT-INF/lib/`**, making the JAR completely self-contained.
