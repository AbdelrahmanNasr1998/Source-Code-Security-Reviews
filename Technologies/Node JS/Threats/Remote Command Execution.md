# ⚠️ **RCE / Dangerous Commands – Javascript (Node.js)**

---

## 🖥️ **1. Direct Command Execution**

**Keywords (child\_process module):**
`child_process.exec`,
`child_process.execSync`,
`child_process.spawn`,
`child_process.spawnSync`,
`child_process.fork`,
`child_process.execFile`,
`child_process.execFileSync`

**👉 Also search for:**

* `process.binding('spawn')` (low-level unsafe use)
* `require('child_process')` (to detect where execution may happen)

---

## 📜 **2. Eval & Dynamic Code Execution**

**Keywords:**
`eval`,
`Function` constructor (`new Function("code")`),
`setTimeout("code", ...)` (string as first arg),
`setInterval("code", ...)`,
`vm.runInThisContext`,
`vm.runInContext`,
`vm.runInNewContext`

**👉 Also search for:**

* `require('vm')` (any unsafe code execution sandboxing)
* `process.mainModule.require` (bypasses restrictions)

---

## 📦 **3. File Inclusion / Module Loading**

**Keywords:**
`require(variable)` (when argument is user-controlled),
`import(variable)` (ES modules, if dynamic),
`fs.readFile` / `fs.readFileSync` (loading user-controlled JS),
`fs.writeFile` + `require` chain (arbitrary file injection)

**👉 Also search for:**

* `require.resolve` (with user input)
* `module.constructor._load`
* `process.binding('natives')`

---

## 🗄️ **4. Deserialization / Insecure Parsing**

**Keywords:**
`JSON.parse` (when user input later passed to eval/dynamic execution),
`yaml.load` (with `js-yaml` – unsafe),
`xml2js.parseString` (XXE + injection risk),
`properties.parse`,
`serialize-javascript` (known vuln if misused),
`node-serialize` (`.unserialize()` leads to RCE in some versions)

**👉 Also search for:**

* `vm.runInNewContext` combined with deserialized objects
* `eval` on parsed JSON/YAML/XML

---

## 🎭 **5. Dangerous Input / Templating Injection**

**Keywords:**
`ejs.render`,
`handlebars.compile`,
`pug.render`,
`mustache.render`,
`nunjucks.render`

**👉 Also search for:**

* `eval` inside templates (Server-Side Template Injection → RCE)
* `with({userInput})` in templating engines
* `res.send(eval(userInput))` patterns

---

## 🌐 **6. HTTP / Dangerous Modules**

**Keywords:**
`express()` (look for `app.use(eval(...))` or user-input passed into `res.render`)
`res.send(eval(...))`
`res.end(Function(...))`

**👉 Also search for:**

* `http-proxy` with unsanitized `target`
* `sandbox-exec` packages misused
* `vm2` (popular sandbox lib with known escapes)

---

## 📚 **7. Dangerous Libraries / Packages (Known to Allow RCE)**

* **`node-serialize`** → `unserialize()` RCE vuln
* **`mongoose`** (NoSQL injection → RCE if chained)
* **`lodash`** / **`underscore`** (prototype pollution → RCE)
* **`jquery`** in Node (remote code injection if misused)
* **`vm2`** sandbox escapes
* **`shelljs`** (wraps `exec`, `execSync`)
* **`systeminformation`** (command injection in old versions)

---

## 🧬 **8. Prototype Pollution**

Prototype pollution lets attackers **inject properties into global object prototypes**, leading to privilege escalation, DoS, or even RCE when combined with dangerous sinks.

**Common sinks / sources:**

* Direct assignments:

  * `obj.__proto__`
  * `obj.constructor.prototype`

* JSON injections:

  ```json
  {   "__proto__": { "polluted": "yes" } }
  ```

* Built-in / language features:

  * `Object.assign(target, userInput)` (unsafe merge)
  * `Object.prototype` manipulation

* Libraries:

  * `_.merge` (Lodash / Underscore)
  * `lodash.merge`
  * `lodash.defaultsDeep`
  * `jquery.extend`
  * `hoek.merge`
  * `mixin-deep`
  * `deep-extend`
  * `merge.recursive`
  * `node.extend`

**Exploitation impact:**

* **Arbitrary property injection** (e.g., adding `isAdmin: true` to all objects)
* **Bypassing security checks**
* **Triggering RCE** when polluted values are later used in `eval`, `Function`, or templating engines
