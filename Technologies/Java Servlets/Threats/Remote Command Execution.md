# ⚡ **RCE / Dangerous Commands – Java Servlets**

---

## 🖥️ **1. Direct OS Command Execution**

**Keywords / APIs:**

* `Runtime.getRuntime().exec(...)`
* `ProcessBuilder` (`new ProcessBuilder(...)`, `.start()`)
* `Process.getInputStream()` / `getErrorStream()` (reading output)
* `Files.write` / `FileOutputStream` chained with `ProcessBuilder` (write a script then exec)

**👉 Also search for:**

* `exec(` together with `request.getParameter`, `request.getHeader`, `cookie` variables
* `cmd.exe`, `powershell`, `sh`, `bash` inside string concat or parameterized args
* `new File("/tmp/"+param)` or `File.createTempFile(...)` + `setExecutable` patterns

---

## 🔮 **2. Reflection & Dynamic Code Execution**

**Keywords / APIs:**

* `Class.forName(userInput)`
* `ClassLoader.loadClass(userInput)` / `URLClassLoader` with user-controlled URLs
* `Method.invoke(...)` / `Constructor.newInstance(...)` invoked inside servlet code
* `ScriptEngineManager` → `engine.eval(userInput)` in servlet/filter/JSP
* `groovy.lang.GroovyShell.evaluate(...)` invoked from a servlet

**👉 Also search for:**

* `new URLClassLoader(new URL[]{ new URL(param) })` in webapp code
* `ServletContext.getAttribute("...")` holding dynamic class names or code
* `javax.tools.JavaCompiler` usage inside servlet contexts

---

## 📦 **3. Deserialization**

**Dangerous Classes / APIs:**

* `ObjectInputStream.readObject(...)` — especially reading from `request.getInputStream()` or cookies
* `XMLDecoder.readObject(...)`, `XStream.fromXML(...)` in servlets
* `Jackson ObjectMapper.readValue` with polymorphic typing enabled (e.g., `enableDefaultTyping`)
* `HttpSession` deserialization (server persists sessions to disk/DB; untrusted object injection)
* `Commons-Collections` gadget chains referenced in servlet libs

**👉 Also search for:**

* `request.getSession().getAttribute(...)` + cast to deserialized type
* `request.getInputStream()` + `new ObjectInputStream(...)`
* `Cookie` values passed into deserialization or `readObject`

---

## 🧩 **4. Expression Injection (EL / OGNL / SpEL / JSP Scriptlets)**

**Keywords / APIs:**

* JSP EL eval contexts: `${param.some}` used unsafely in includes or `jsp:include`
* `ExpressionLanguage` / `javax.el.ExpressionFactory` / `ELProcessor` evaluated in servlets/JSP
* `Ognl.parseExpression` / `MVEL.eval` / `JEXL` used from servlet code
* Raw scriptlets (`<% %>`) or `out.print()` of unescaped user input inside JSPs

**👉 Also search for:**

* `RequestDispatcher.include(request,response)` where the included path is derived from input
* `response.getWriter().write(userInput)` inside JSP without escaping
* `th:utext`-style patterns in mixed JSP/Thymeleaf setups (if present)

---

## 📦 **5. File Inclusion, Uploads & Class Loading**

**Keywords / APIs:**

* `request.getRequestDispatcher(userInput).forward(...)` / `include(...)` with untrusted paths
* `ServletContext.getResourceAsStream(userInput)` / `getRealPath(userInput)` used with input
* `Part.write(...)` (Servlet 3.0 `Part`) or Apache Commons FileUpload → `FileItem.write(...)`
* Saving uploaded jars/war files to webapp directories then relying on auto-deploy/reload

**👉 Also search for:**

* `new URL("file://..."+param)` or `new URL(param)` used to fetch classes or resources
* `File.renameTo` / `Files.move` moving uploaded content into `WEB-INF/classes` or `WEB-INF/lib`
* `ServletContext.getClassLoader()` usage followed by `loadClass(...)`

---

## 🌐 **6. HTTP → Dangerous Execution / Chaining**

**Patterns:**

* `request.getParameter`, `request.getHeader`, `request.getCookies`, `request.getInputStream()` → used directly in `exec`, reflection, eval, or file paths
* File upload (`multipart/form-data`) → write to disk → load as class/jar or call `Runtime.exec` on it
* SSRF from servlet (e.g., `HttpURLConnection` built from param) → fetch payload and `URLClassLoader` it

**👉 Also search for:**

* `request.getParameter("cmd")`, `request.getParameterMap()`, `Enumeration<String> params` loops
* `response.getWriter().println(param)` used for debugging that may leak payloads or trigger log-based attacks

---

## 📌 **7. Dangerous Libraries / Features in Servlet Environments**

* **Java Serialization / HttpSession persistence** (containers that persist sessions can be exploited)
* **Apache Commons FileUpload** (`FileItem.write`) — check for unsafe filename handling / path traversal
* **Log4j2** (Log4Shell) used inside servlets — user input logged without validation
* **JNDI / InitialContext.lookup(userInput)** called from servlets/filters
* **Groovy / Nashorn / ScriptEngine** invoked from webapp code
* **Apache Commons BeanUtils** (`PropertyUtils.setProperty`) used to populate beans from `request.getParameterMap()`
* **JSPs with dynamic includes or scriptlets** that evaluate untrusted input

---

## 🔎 **Quick Search Patterns / Sinks to Look For in Codebase**

* `request.getParameter(`, `request.getHeader(`, `request.getCookies(`, `request.getInputStream(`
* `new ObjectInputStream(`, `XMLDecoder(`, `XStream.`
* `Runtime.getRuntime().exec(`, `new ProcessBuilder(`, `.start()`
* `Class.forName(`, `getClassLoader().loadClass(`, `new URLClassLoader(`
* `ScriptEngineManager(`, `engine.eval(`, `GroovyShell.evaluate(`
* `request.getPart(`, `Part.write(`, `FileItem.write(`, `ServletFileUpload`
* `RequestDispatcher.forward(`, `RequestDispatcher.include(`, `getRequestDispatcher(` with param-based paths
* `InitialContext.lookup(`, `JndiTemplate.lookup(`
* `response.getWriter().write(` / `out.print(` with user input in JSPs
* `session.setAttribute(` / `session.getAttribute(` around serialized objects

---

## ✅ **Quick Mitigations & Notes (copy/paste friendly)**

* Never pass raw `request` input into `exec`, reflection, `eval`, class loading, or deserialization. Always validate & whitelist.
* Avoid `ObjectInputStream` on untrusted streams; use safe formats (JSON with restricted types). Disable polymorphic typing in Jackson.
* Sanitize filenames and avoid writing uploads into executable webapp directories. Store uploads outside webroot and scan content.
* Remove or sandbox script engines; disable JNDI lookups for untrusted URIs. Upgrade Log4j and ensure no user-controlled input reaches logging lookup patterns.
* Treat `HttpSession` persistence as a high-risk sink — harden container configs or disable cross-restart session persistence for untrusted environments.
