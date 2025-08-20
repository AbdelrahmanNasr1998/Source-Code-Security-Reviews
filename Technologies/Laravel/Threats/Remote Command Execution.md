# ⚠️ **RCE / Dangerous Commands – PHP (Laravel)**

---

## 🖥️ **1. Direct Command Execution**

**Keywords:**
`exec`,
`shell_exec`,
`system`,
`passthru`,
`popen`,
`proc_open`

**👉 Also search for:**

* `pcntl_exec` (spawns processes)
* `expect_popen` (if Expect extension is enabled)
* `dl()` (load malicious extensions dynamically)
* `mail()` (can sometimes be abused if arguments are not sanitized)

---

## 📜 **2. Eval & Dynamic Code Execution**

**Keywords:**
`eval`,
`assert` (with string input),
`create_function` (PHP <7.2, internally uses eval),
`preg_replace` with `/e` modifier (PHP <5.5, eval injection),
`mb_ereg_replace` with `/e` modifier

**👉 Also search for:**

* `ReflectionFunction::invoke`
* `ReflectionMethod::invoke`
* `call_user_func` / `call_user_func_array` (if passed untrusted input)
* `forward_static_call` / `forward_static_call_array`
* `runkit_function_add` / `runkit_import` (Runkit extension – rare but dangerous)

---

## 📂 **3. File Inclusion (LFI/RFI → RCE)**

**Keywords:**
`include`,
`include_once`,
`require`,
`require_once`

**👉 Also search for:**

* `virtual()` (Apache-specific, can include files)
* `parse_ini_file` + `include` chain
* `readfile` → if combined with uploads + eval

---

## 📦 **4. Deserialization / Insecure Loading**

**Keywords:**
`unserialize`,
`yaml_parse`, `yaml_parse_file` (YAML extension),
`SimpleXMLLoadString`, `SimpleXMLLoadFile`,
`DOMDocument::loadXML`,
`DOMDocument::loadHTML`

**👉 Also search for:**

* `Phar::load` / `phar://` stream wrappers (Phar deserialization)
* `wddx_deserialize`
* `msgpack_unpack` (if MsgPack extension enabled)
* `igbinary_unserialize` (if Igbinary extension enabled)

**Laravel-Specific Additions:**

* **`unserialize()` inside Laravel apps** → especially in:

  * `Illuminate\Queue\CallQueuedHandler`
  * `Illuminate\Broadcasting\PendingBroadcast`
  * `Illuminate\Notifications\Channels\DatabaseChannel`

  ⚠️ These are known deserialization gadget chains for RCE if attacker controls serialized input.

* **`Facade::unserialize` misuse** (very rare but possible).

* **Laravel Queue / Job serialization** → Dangerous if user input is serialized/deserialized.

---

## 🛑 **5. Dangerous Input / Execution Functions**

**Keywords:**
`eval` (again, most common),
`assert` (string form),
`preg_replace('/e/')`,
`parse_str` (if used with untrusted input → variable injection),
`extract` (with untrusted arrays),
`mb_parse_str`

**👉 Also search for:**

* `$GLOBALS` manipulation with user input
* `variable variables` (`$$var` patterns)
* `create_function` (deprecated but still in legacy code)

**Laravel-Specific Additions:**

* `Artisan::call($userInput)` → can trigger arbitrary artisan commands.
* `Route::any(..., function($input) { eval($input); })` → (common in CTF/buggy code).
* `Blade::compileString($userInput)` → can lead to template injection → RCE.
* `view($userInput)` if `$userInput` is directly attacker-controlled → LFI/RCE.

---

## 🌐 **6. Wrappers & Streams**

PHP supports many **wrappers** that can be abused to load or execute content:

**Keywords:**
`php://input`,
`php://filter`,
`php://stdin`,
`data://`,
`expect://`,
`phar://`,
`zip://`,
`rar://`

**👉 Also search for:**

* `allow_url_include` (if enabled → RFI risk)
* `fopen` / `file_get_contents` with remote URLs
* `curl_exec` (if response passed into eval/include)

---

## 📚 **7. PHP – Dangerous Libraries / Packages (Known to Allow RCE)**

1. **Unserialize-related**:

   * `unserialize()` with user input (classic RCE if gadgets exist).
   * Libraries with “POP chains”:

     * **Monolog**
     * **SwiftMailer**
     * **Symfony** (older versions)
     * **Drupal** (deserialization bugs)
     * **TYPO3**

2. **Templating engines**:

   * **Smarty** (`eval`-based template compilation, SSTI possible).
   * **Twig** (unsafe if `{{ user_input|raw }}` or sandbox disabled).

3. **Command execution wrappers**:

   * **Symfony Process Component** (`Process(user_input)`)
   * **Laravel Artisan Commands** (if dynamically constructed from input)

4. **XML libraries**:

   * `simplexml_load_string`, `DOMDocument::loadXML`, `XMLReader` (XXE → possible RCE if chained).

5. **Deserialization in frameworks**:

   * **CodeIgniter** (known unserialize gadget chains)
   * **Zend Framework** (older versions had RCE gadgets)

6. **Image libraries**:

   * **ImageMagick via PHP (imagick extension)** → "ImageTragick" RCE.
   * **GD with malicious inputs** (heap corruption → exploitable in some cases).

7. **Other dangerous packages / functions**:

   * `PHPMailer` (older versions → RCE via crafted headers)
   * `PHPExcel` (XXE and deserialization bugs)
   * `TCPDF` (code injection bugs in old versions)

---

## 🎯 **8. Laravel-Specific Dangerous Features**

**1. Blade Template Injection (SSTI → RCE)**

* `{{ $userInput }}` in Blade **is safe** (auto-escaped).
* `{!! $userInput !!}` (raw echo) = **dangerous**, can lead to XSS or RCE if attacker can inject Blade syntax.
* `@php eval($userInput) @endphp` blocks = direct RCE.

**2. Deserialization via Laravel Cookies / Sessions**

* Laravel’s `EncryptCookies` middleware uses `unserialize()` internally.
* If `APP_KEY` is weak/leaked → attacker can forge cookies → RCE via deserialization gadgets.

**3. Remote File Inclusion via Storage/Config**

* `Storage::disk('local')->get($userInput)` → if combined with `include`.
* `config($userInput)` → could lead to loading attacker-controlled configs.

**4. Dangerous Laravel Helpers**

* `app()->make($userInput)` → instantiate arbitrary classes.
* `resolve($userInput)` → resolve arbitrary classes.
* `call_user_func($userInput)` (if used with request data in Laravel controllers).
