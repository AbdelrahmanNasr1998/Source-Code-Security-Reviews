# ⚡ **RCE / Dangerous Commands – Java (Spring Boot)**

---

## 🖥️ **1. Direct OS Command Execution**

**Keywords / APIs:**

* `Runtime.getRuntime().exec(...)`
* `ProcessBuilder` (`new ProcessBuilder(...)`)
* `Process.start()`
* `System.getProperty("os.name")` + conditional exec

**👉 Also search for:**

* `sh`, `bash`, `cmd.exe`, `powershell` inside exec arguments
* `Files.write` / `FileOutputStream` + `ProcessBuilder` (malicious chaining)

---

## 🔮 **2. Reflection & Dynamic Code Execution**

**Keywords / APIs:**

* `Class.forName(userInput)`
* `ClassLoader.loadClass(userInput)`
* `Method.invoke(...)`
* `Constructor.newInstance(...)`
* `GroovyShell.evaluate(...)`
* `ScriptEngineManager` → (`engine.eval(userInput)`)

**👉 Also search for:**

* `javax.tools.JavaCompiler` (`compile` on untrusted source)
* `sun.misc.Unsafe` (arbitrary memory ops → exploited in RCE chains)

---

## 📦 **3. Deserialization**

**Dangerous Classes / APIs:**

* `ObjectInputStream.readObject`
* `XMLDecoder.readObject`
* `XStream.fromXML`
* `Jackson ObjectMapper.readValue` (when `enableDefaultTyping` is on)
* `Gson.fromJson` (unsafe polymorphic types)
* `Kryo.readClassAndObject`
* `HessianInput.readObject`

**👉 Also search for:**

* `Commons-Collections` (gadgets used in deserialization RCEs)
* `Spring Beans XML deserialization`
* `Java Serialization API` (`Serializable` + untrusted source)

---

## 🧩 **4. Expression Injection (Spring EL / OGNL / SpEL)**

**Keywords / APIs:**

* `@Value("#{...}")` (SpEL injection)
* `ExpressionParser.parseExpression(userInput)`
* `StandardEvaluationContext` with untrusted input
* `Ognl.parseExpression`
* `MVEL.eval`
* `JEXL` (`JexlEngine.createExpression(userInput)`)

**👉 Also search for:**

* JSP EL (`${...}`) if unescaped
* Thymeleaf template expressions with `th:utext="${userInput}"`

---

## 📚 **5. File Inclusion & Class Loading**

**Keywords / APIs:**

* `URLClassLoader` (`new URLClassLoader(new URL[]{ new URL(userInput) })`)
* `ClassLoader.defineClass`
* `JarFile` + `loadClass`
* `Files.newInputStream(Paths.get(userInput))` with dynamic class loading
* `Spring Boot DevTools` → loading remote jars in some misconfigs

**👉 Also search for:**

* `ScriptEngineManager.getEngineByName("js")` → JavaScript eval
* `GroovyClassLoader.parseClass(userInput)`

---

## 🌐 **6. HTTP → Dangerous Execution**

**Patterns:**

* `@RequestParam`, `@RequestBody`, `@PathVariable` → directly into `exec`, reflection, or eval
* `MultipartFile` → written to disk + loaded as class/jar
* `RestTemplate.exchange` → SSRF → chaining into RCE

**👉 Also search for:**

* `response.getWriter().write(userInput)`
* `request.getParameter("...")` → passed to dangerous sinks

---

## 📌 **7. Dangerous Libraries / Features**

* **Spring Core** (e.g., *Spring4Shell* – `ClassLoader` manipulation via `@Value`)
* **Apache Commons BeanUtils** (`PropertyUtils.setProperty`)
* **Apache Commons JEXL / OGNL** → expression injection
* **Groovy** (`GroovyShell`, `Eval`)
* **Nashorn JavaScript Engine** (`engine.eval`)
* **Log4j2 JNDI injection** (Log4Shell RCE)
* **JNDI lookups:**

  * `InitialContext.lookup(userInput)`
  * `JndiTemplate.lookup(userInput)`
