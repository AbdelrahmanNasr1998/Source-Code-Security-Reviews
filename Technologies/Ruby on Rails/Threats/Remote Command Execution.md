# ⚠️ **Ruby / Ruby on Rails – RCE & Dangerous Features**

---

## 🚀 **1. Direct Command Execution (Ruby Core)**

**Keywords (search for these in code):**

* `system`
* `exec`
* `` `backticks` ``
* `%x{}` (same as backticks)
* `IO.popen`
* `Open3.popen2`, `Open3.popen3`
* `Kernel.spawn`

**👉 Also search for:**

* `Process.spawn`, `Process.exec`
* `PTY.spawn` (pseudo-terminal execution)
* `eval` with user input
* `instance_eval`, `class_eval`, `module_eval` (dynamic code execution)

---

## 🔮 **2. Dynamic Code Execution**

**Dangerous Ruby methods:**

* `eval`
* `send` / `__send__` (can invoke private methods if attacker controls input)
* `method_missing` (dynamic method dispatch, can be abused)
* `define_method` (with user input)
* `binding.eval` (gives access to execution context)

**Rails-specific:**

* `render inline: params[:template]` (ERB injection → RCE)
* `render :text => user_input, :inline => true`

---

## 📂 **3. File Inclusion / Loading**

* `require user_input`
* `load user_input`
* `autoload` (if controlled by attacker input)

**Rails-specific:**

* `render file: params[:file]` (LFI → RCE if attacker can upload files)
* `render params[:template]` (SSTI → ERB injection)

---

## 📦 **4. Deserialization / YAML / Marshal**

**Dangerous parsing functions:**

* `YAML.load`
* `YAML.load_file`
* `YAML.parse`
* `Marshal.load`
* `Oj.load` (if mode not restricted)

👉 Safer alternatives:

* `YAML.safe_load`
* `Oj.safe_load`

**Rails-specific risks:**

* Rails historically had RCE via `YAML.load` in cookie/session handling.
* If `secret_key_base` is weak/leaked → forged session → RCE gadget chains.

---

## 🛠️ **5. Insecure Mass Assignment**

* Before Rails 4, models allowed **mass assignment**:

```ruby
User.new(params[:user])
```

If not restricted, attacker could set arbitrary fields (e.g., `is_admin = true`).
While not direct RCE, it can lead to privilege escalation → RCE later.

---

## 🔎 **6. Dangerous Helpers / Reflection**

* `constantize` (`params[:class_name].constantize.new`) → attacker controls class instantiation.
* `constant_get`, `Object.const_get`
* `public_send` (if attacker controls method name)
* `send(:eval, user_input)` chaining
* `Kernel#load`

---

## 🏗️ **7. Dangerous Rails Features**

1. **ERB / HAML / Slim Template Injection**

   * `<%= user_input %>` is safe (escaped by default).
   * `<%== user_input %>` or `raw(user_input)` is dangerous.
   * `eval` inside templates = direct RCE.

2. **ActiveSupport::Dependencies**

   * `constantize`, `safe_constantize`, `constant_get` on user input.

3. **ActiveRecord::find\_by\_sql**

   * Direct SQL injection risk (not RCE, but can pivot to RCE).

4. **Backticks inside Rails code**

   * `` `#{params[:cmd]}` `` → instant RCE.

---

## 📚 **8. Dangerous Libraries / Gems (Known RCE Risk)**

1. **Deserialization gadgets**

   * `YAML.load` + **Rails**, **ActiveSupport** objects.
   * `Marshal.load` with attacker input.

2. **Templating engines**

   * **ERB** (`<%= eval user_input %>`)
   * **Slim** / **HAML** (if raw input is injected)

3. **Serialization / Background job libraries**

   * **Resque**, **Sidekiq**, **Delayed::Job** (insecure job data serialization).
   * **ActiveJob** with custom serializers.

4. **PDF / File processing gems**

   * **Paperclip** (ImageMagick injection → ImageTragick RCE)
   * **CarrierWave** (unsafe file handling → arbitrary file upload → RCE)
   * **MiniMagick** (wrapper around ImageMagick)

5. **Mail libraries**

   * **ActionMailer** if headers are attacker-controlled (older versions had RCE via crafted headers).

6. **Other risky gems**

   * `Oj` (unsafe load)
   * `Psych` (`YAML.load` backend)
   * `Erubis` (eval-based rendering)
