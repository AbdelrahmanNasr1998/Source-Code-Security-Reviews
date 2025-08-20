# 📒 **RCE / Dangerous Commands – Python (Flask)**

---

## ⚡ 1. OS Command Execution

**🔑 Keywords:**
`os.system`,
`os.popen`, `os.popen2`, `os.popen3`, `os.popen4`,
`os.execv`, `os.execve`, `os.execvp`, `os.execvpe`,
`os.spawn*` (`spawnl`, `spawnlp`, `spawnv`, `spawnvp`, …),
`commands.getoutput`, `commands.getstatusoutput` (Python 2),
`pty.spawn`

**🔍 Also search for:**

* `sh` module (`sh.ls`, `sh.cat`, etc. → wraps shell commands)
* `plumbum.commands`
* `fabric.api.local` / `fabric.api.run`
* `pexpect.spawn`

---

## ⚡ 2. Subprocess Module

**🔑 Keywords:**
`subprocess.call`,
`subprocess.run`,
`subprocess.Popen`,
`subprocess.check_call`,
`subprocess.check_output`,
`subprocess.getoutput`,
`subprocess.getstatusoutput`

**🔍 Also search for:**

* `shell=True` in `subprocess.*`
* `shlex.split`
* Wrappers like `invoke.run`

---

## ⚡ 3. Code Execution / Dynamic Eval

**🔑 Keywords:**
`eval`,
`exec`,
`execfile` (Python 2),
`compile`,
`runpy.run_module`, `runpy.run_path`,
`__import__`,
`getattr`,
`setattr`

**🔍 Also search for:**

* `globals()`, `locals()`
* `vars()`
* `types.FunctionType`
* `inspect.getsource` + `exec`

---

## ⚡ 4. Deserialization / Insecure Loading

**🔑 Keywords:**
`pickle.load`, `pickle.loads`,
`cPickle.load`, `cPickle.loads`,
`marshal.load`, `marshal.loads`,
`yaml.load` (unsafe),
`dill.load`, `dill.loads`,
`cloudpickle.load`, `cloudpickle.loads`,
`joblib.load`

**🔍 Also search for:**

* `shelve.open`
* `anyconfig.load`
* `jsonpickle.decode`
* `tensorflow.saved_model.load`

---

## ⚡ 5. Dangerous Input Functions

**🔑 Keywords:**
`input` (Python 2 → unsafe eval),
`raw_input` (if passed to eval/exec),
`ast.literal_eval`,
`code.interact`

**🔍 Also search for:**

* `cmd.Cmd`
* `IPython.embed`
* `code.compile_command`
* `runpy._run_module_as_main`

---

## 🐍 6. Flask – Dangerous Patterns

👉 **Flask apps often introduce RCE through template engines, deserialization, or extensions:**

* **📝 Jinja2 Template Injection**

  * `render_template_string(user_input)`
  * Passing unsanitized data into `render_template` context when attacker controls Jinja syntax

* **📦 Pickle in Flask Sessions**

  * `Flask(app, session_interface=PickleSessionInterface)`
  * Any `pickle.loads` used for session handling

* **🐞 Werkzeug Debug Console**

  * If `app.debug = True` and debugger exposed → remote Python code execution

* **⚠️ Unsafe Extensions / Patterns**

  * `itsdangerous` misuse (weak secret for signing data)
  * Custom deserialization of cookies or JSON (e.g., `jsonpickle`)

---

## 🐍 7. Flask – Dangerous Libraries / Features

**1️⃣ Template Injection → RCE**

* Flask’s Jinja2 (`render_template`, `render_template_string`)
* User input passed unsafely → sandbox escape → RCE

**2️⃣ Deserialization / Dangerous Libraries**

* `pickle.loads`, `yaml.load`, `dill.load` in routes/extensions
* `itsdangerous` (weak secret keys)
* `werkzeug.debug` console in production

**3️⃣ Dangerous Utilities**

* Direct `os.system`, `subprocess.*`, `eval`, `exec` in routes/blueprints
* `flask.ext.shell` or `code.interact`
