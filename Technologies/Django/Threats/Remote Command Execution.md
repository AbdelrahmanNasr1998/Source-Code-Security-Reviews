---

# ⚡️ **RCE / Dangerous Commands – Python (Django)**

---

## 🖥️ **1. OS Command Execution**

**Keywords:**  
`os.system`,  
`os.popen`, `os.popen2`, `os.popen3`, `os.popen4`,  
`os.execv`, `os.execve`, `os.execvp`, `os.execvpe`,  
`os.spawn*` (`spawnl`, `spawnlp`, `spawnv`, `spawnvp`, …),  
`commands.getoutput`, `commands.getstatusoutput` (Python 2),  
`pty.spawn`

**🔍 Also search for:**

- `sh` module (`sh.ls`, `sh.cat`, etc. → wraps shell commands)  
- `plumbum.commands`  
- `fabric.api.local` / `fabric.api.run`  
- `pexpect.spawn`  

---

## ⚙️ **2. Subprocess Module**

**Keywords:**  
`subprocess.call`,  
`subprocess.run`,  
`subprocess.Popen`,  
`subprocess.check_call`,  
`subprocess.check_output`,  
`subprocess.getoutput`,  
`subprocess.getstatusoutput`

**🔍 Also search for:**

- `shell=True` in `subprocess.*`  
- `shlex.split`  
- Wrappers like `invoke.run`  

---

## 📜 **3. Code Execution / Dynamic Eval**

**Keywords:**  
`eval`,  
`exec`,  
`execfile` (Python 2),  
`compile`,  
`runpy.run_module`, `runpy.run_path`,  
`__import__`,  
`getattr`,  
`setattr`

**🔍 Also search for:**

- `globals()`, `locals()`  
- `vars()`  
- `types.FunctionType`  
- `inspect.getsource` + `exec`  

---

## 📦 **4. Deserialization / Insecure Loading**

**Keywords:**  
`pickle.load`, `pickle.loads`,  
`cPickle.load`, `cPickle.loads`,  
`marshal.load`, `marshal.loads`,  
`yaml.load` (unsafe),  
`dill.load`, `dill.loads`,  
`cloudpickle.load`, `cloudpickle.loads`,  
`joblib.load`

**🔍 Also search for:**

- `shelve.open`  
- `anyconfig.load`  
- `jsonpickle.decode`  
- `tensorflow.saved_model.load`  

---

## 📝 **5. Dangerous Input Functions**

**Keywords:**  
`input` (Python 2 → unsafe eval),  
`raw_input` (if passed to eval/exec),  
`ast.literal_eval`,  
`code.interact`

**🔍 Also search for:**

- `cmd.Cmd`  
- `IPython.embed`  
- `code.compile_command`  
- `runpy._run_module_as_main`  

---

## 🏗️ **6. Django-Specific Dangerous Patterns**

- **Template Injection**
  - `Template(user_input)`  
  - `Engine.from_string(user_input)`  
  - `loader.get_template_from_string` (older versions)  
  - Any custom template rendering where user input = template source  

- **ORM / Raw SQL Execution**
  - `Model.objects.raw("SELECT ... %s" % user_input)`  
  - `cursor.execute(user_input)` (direct DB cursor)  

- **Management Commands / Shells**
  - `call_command(user_input)` (with dynamic input)  
  - `exec(open("manage.py").read())` (rare but seen in bad code)  

- **Dangerous Settings / Middleware**
  - `DEBUG = True` + exposed stack traces → remote code execution via template debug  
  - Insecure custom middleware that uses `eval`  

---

## 🚨 **7. Dangerous Libraries / Features – Django**

**1. Template Injection → RCE**  
- `django.template.Engine` (if misconfigured with `debug=True` or unsafe template contexts).  
- Usage of **Jinja2** instead of Django templates inside Django projects → unsafe if user input goes directly into templates.  

**2. Serialization / Deserialization**  
- `django.core.serializers` (loading untrusted data).  
- `pickle` usage inside Django views/models (common in legacy projects).  
- `django.core.signing.loads` (if secret key is weak/leaked).  

**3. Dangerous Utilities**  
- `os.system`, `subprocess.*` inside views, tasks, or management commands.  
- `eval`, `exec`, `compile` used in custom Django apps.  

---
