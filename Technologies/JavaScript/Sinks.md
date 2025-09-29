# 🔐 JavaScript Security Review Checklist

Here's a comprehensive breakdown of each security sink with analysis guidance and visual indicators:

## 🚨 **Code-Evaluation / Dynamic Code Execution**

### ⚠️ **Critical Severity**
```javascript
// DANGEROUS PATTERNS
eval(userInput)
new Function('return ' + userData)
setTimeout(userControlledString, 100)
setInterval(untrustedCodeString, 100)
vm.runInNewContext(userCode)
```
**🔍 Analysis Checklist:**
- [ ] Search for all `eval()` calls
- [ ] Check `Function` constructor usage
- [ ] Review `setTimeout/setInterval` with strings
- [ ] Verify Node.js `vm` module usage
- [ ] Look for indirect eval `(0, eval)()`

**✅ Safe Alternatives:**
- Use JSON.parse() for data
- Use first-class functions
- Implement sandboxed environments

---

## 🌐 **DOM HTML Insertion (Client-Side XSS Sinks)**

### ⚠️ **Critical Severity**
```javascript
// UNSAFE SINKS
element.innerHTML = userContent
element.outerHTML = userContent
document.write(userContent)
document.writeln(userContent)
$(element).html(userContent)
```
**🔍 Analysis Checklist:**
- [ ] Audit all `innerHTML/outerHTML` assignments
- [ ] Check jQuery `.html()` `.append()` methods
- [ ] Review `document.write/writeln` usage
- [ ] Verify DOMPurify or other sanitizers
- [ ] Check for unsafe React `dangerouslySetInnerHTML`

**✅ Safe Alternatives:**
```javascript
element.textContent = safeText
element.setAttribute('data-safe', value)
React: Use proper JSX escaping
```

---

## ⚡ **Event Handler / Attribute Injection**

### ⚠️ **High Severity**
```javascript
// RISKY PATTERNS
element.setAttribute('onclick', userData)
element.onclick = () => eval(userData)
<img src="x" onerror="<%= userContent %>">
```
**🔍 Analysis Checklist:**
- [ ] Review dynamic event handler assignment
- [ ] Check attribute setters with user data
- [ ] Audit template literals in attributes
- [ ] Verify SVG event handlers
- [ ] Review `data-*` attribute usage

**✅ Safe Practices:**
- Use `.addEventListener()` with functions
- HTML-encode attribute values
- Validate URL protocols in `src/href`

---

## 📝 **Template Engine Rendering & String Templates**

### ⚠️ **High Severity**
```javascript
// VULNERABLE TEMPLATES
const template = `<div>${userInput}</div>`
res.render('template', { userData: unsanitized })
_.template(userControlledTemplate)
```
**🔍 Analysis Checklist:**
- [ ] Review template literal usage with user data
- [ ] Check server-side template engines (EJS, Pug, Handlebars)
- [ ] Verify auto-escaping is enabled
- [ ] Audit dynamic template loading
- [ ] Review string concatenation in templates

**✅ Safe Practices:**
- Enable auto-escaping in template engines
- Use template helpers for sanitization
- Implement Content Security Policy

---

## 🗄️ **Server-Side/DB Query Concatenation (SQL Injection)**

### ⚠️ **Critical Severity**
```javascript
// SQL INJECTION RISKS
db.query(`SELECT * FROM users WHERE id = ${userId}`)
knex.raw(`WHERE name = '${name}'`)
`INSERT INTO table VALUES (${value})`
```
**🔍 Analysis Checklist:**
- [ ] Search for string concatenation in SQL queries
- [ ] Review raw query usage
- [ ] Check ORM raw query methods
- [ ] Verify parameterized queries
- [ ] Audit dynamic table/column names

**✅ Safe Practices:**
```javascript
// USE PARAMETERIZED QUERIES
db.query('SELECT * FROM users WHERE id = ?', [userId])
knex.where('name', name)
```

---

## 🍃 **NoSQL / MongoDB / Query Object Injection**

### ⚠️ **High Severity**
```javascript
// NOSQL INJECTION
User.find({ username: req.body.username })
db.collection.find({ $where: userInput })
User.update({ _id: id }, { $set: userData })
```
**🔍 Analysis Checklist:**
- [ ] Review direct user input in query objects
- [ ] Check `$where` operator usage
- [ ] Audit aggregation pipelines with user data
- [ ] Verify input validation for query operators
- [ ] Review update operations with user objects

**✅ Safe Practices:**
- Validate and sanitize query objects
- Use schema validation (Mongoose)
- Implement operator whitelisting

---

## 💻 **Command Execution / OS Injection (Node)**

### ⚠️ **Critical Severity**
```javascript
// COMMAND INJECTION
child_process.exec(`ls ${userDir}`)
child_process.spawn('git', ['clone', userUrl])
execSync(`npm install ${userPackage}`)
```
**🔍 Analysis Checklist:**
- [ ] Audit `child_process` module usage
- [ ] Check for shell command concatenation
- [ ] Review `spawn` with `shell: true`
- [ ] Verify input validation for file paths
- [ ] Check for unsafe environment variables

**✅ Safe Practices:**
```javascript
// USE execFile WITHOUT SHELL
execFile('ls', [userDir], { shell: false })
// OR validate and sanitize inputs
```

---

## 📦 **Dynamic require() / Module Loading**

### ⚠️ **High Severity**
```javascript
// UNSAFE MODULE LOADING
require(userSuppliedPath)
import(userDynamicModule)
fs.readFileSync(require.resolve(userPath))
```
**🔍 Analysis Checklist:**
- [ ] Search for dynamic `require()` calls
- [ ] Check dynamic `import()` expressions
- [ ] Review module resolution with user input
- [ ] Verify path traversal prevention
- [ ] Audit template inclusion mechanisms

**✅ Safe Practices:**
- Use static imports when possible
- Implement module whitelisting
- Validate and sanitize module paths

---

## 📁 **Filesystem Path Traversal / File Write**

### ⚠️ **High Severity**
```javascript
// PATH TRAVERSAL
fs.readFile(`/uploads/${userFile}`)
fs.writeFile(userPath, data)
res.sendFile(userRequestedFile)
```
**🔍 Analysis Checklist:**
- [ ] Review file operations with user input
- [ ] Check path concatenation
- [ ] Audit file upload handlers
- [ ] Verify path normalization
- [ ] Review static file serving

**✅ Safe Practices:**
- Use path resolution libraries
- Implement file extension whitelisting
- Sanitize file names and paths

---

## 🌐 **HTTP Client / SSRF Sinks**

### ⚠️ **High Severity**
```javascript
// SSRF VULNERABILITIES
fetch(userControlledUrl)
axios.get(req.query.url)
http.get(internalUrl + userPath)
```
**🔍 Analysis Checklist:**
- [ ] Audit HTTP client calls with user URLs
- [ ] Check for internal network access
- [ ] Review URL validation logic
- [ ] Verify protocol restrictions
- [ ] Check for DNS rebinding protection

**✅ Safe Practices:**
- Validate and whitelist allowed domains
- Use URL parsers to check protocols
- Implement network segmentation

---

## 🔗 **Open Redirect / URL Manipulation**

### ⚠️ **Medium Severity**
```javascript
// OPEN REDIRECTS
res.redirect(req.query.returnUrl)
window.location.href = userParam
<a href="<%= returnUrl %>">Back</a>
```
**🔍 Analysis Checklist:**
- [ ] Review all redirect mechanisms
- [ ] Check URL validation before redirects
- [ ] Audit `window.location` assignments
- [ ] Verify same-origin checks
- [ ] Review login/logout redirects

**✅ Safe Practices:**
- Validate redirect URLs against whitelist
- Use relative URLs for internal redirects
- Implement referrer checks

---

## 📋 **HTTP Header / Response Splitting (CRLF Injection)**

### ⚠️ **High Severity**
```javascript
// CRLF INJECTION
res.setHeader('Location', userInput)
res.writeHead(302, { Location: userUrl })
cookie.set('user', userName)
```
**🔍 Analysis Checklist:**
- [ ] Review HTTP header setting with user data
- [ ] Check cookie value assignment
- [ ] Audit response header manipulation
- [ ] Verify header value sanitization
- [ ] Review logging with user input

**✅ Safe Practices:**
- Validate header values for CRLF characters
- Use framework-specific header methods
- Encode user data in headers

---

## ⚛️ **Template/JSX Server-Side Code Injection (SSR)**

### ⚠️ **High Severity**
```javascript
// SSR INJECTION
ReactDOMServer.renderToString(<UserComponent data={userData} />)
vueServerRenderer.renderToString(app)
res.render('component', { unsafeData })
```
**🔍 Analysis Checklist:**
- [ ] Review SSR with user-provided data
- [ ] Check for unsafe prop passing
- [ ] Audit dynamic component rendering
- [ ] Verify proper JSX escaping
- [ ] Review hydration data serialization

**✅ Safe Practices:**
- Sanitize data before SSR
- Use framework-safe rendering methods
- Implement CSP for SSR

---

## 🎯 **Unsafe Deserialization / Object Construction**

### ⚠️ **Critical Severity**
```javascript
// UNSAFE DESERIALIZATION
JSON.parse(untrustedString)
Object.assign(target, userObject)
const obj = { ...userData, ...internalData }
```
**🔍 Analysis Checklist:**
- [ ] Review `JSON.parse()` with untrusted data
- [ ] Check object merging with user input
- [ ] Audit `Object.assign()` and spread operators
- [ ] Verify prototype pollution prevention
- [ ] Review custom deserialization logic

**✅ Safe Practices:**
- Use safe JSON parsers (like `JSON.parse` with validation)
- Implement schema validation
- Use object creation whitelists

---

## 🔄 **Prototype Pollution Sinks**

### ⚠️ **High Severity**
```javascript
// PROTOTYPE POLLUTION
Object.merge(target, userInput)
_.merge({}, userObject)
lib.deepCopy(userData)
```
**🔍 Analysis Checklist:**
- [ ] Review object merging functions
- [ ] Check deep copy operations
- [ ] Audit property assignment with user keys
- [ ] Verify `__proto__` and `constructor` protection
- [ ] Review library-specific merge functions

**✅ Safe Practices:**
- Use safe merge libraries
- Validate object keys
- Freeze built-in prototypes in development

---

## 📨 **Cross-Context / postMessage Sinks**

### ⚠️ **Medium Severity**
```javascript
// UNSAFE POSTMESSAGE
window.addEventListener('message', (event) => {
  element.innerHTML = event.data // UNSAFE!
})
```
**🔍 Analysis Checklist:**
- [ ] Review `message` event listeners
- [ ] Check for origin validation
- [ ] Audit data handling from postMessage
- [ ] Verify target origin in `postMessage` calls
- [ ] Review cross-frame communication

**✅ Safe Practices:**
```javascript
window.addEventListener('message', (event) => {
  if (event.origin !== 'https://trusted.com') return
  // Process data safely
})
```

---

## 🎨 **CSS / Style Sinks**

### ⚠️ **Medium Severity**
```javascript
// CSS INJECTION
element.style = userCss
element.setAttribute('style', userStyle)
$(element).css('background', userColor)
```
**🔍 Analysis Checklist:**
- [ ] Review dynamic style assignment
- [ ] Check for CSS URL injection
- [ ] Audit style attribute manipulation
- [ ] Verify CSS value sanitization
- [ ] Review JavaScript CSS frameworks

**✅ Safe Practices:**
- Use CSS classes instead of inline styles
- Validate CSS values and URLs
- Implement style sanitizers

---

## 📊 **Logging / Telemetry Sinks (Log Injection)**

### ⚠️ **Low Severity**
```javascript
// LOG INJECTION
console.log(userInput)
logger.info(`User action: ${userData}`)
appInsights.trackEvent(userEvent)
```
**🔍 Analysis Checklist:**
- [ ] Review logging with user data
- [ ] Check log formatting
- [ ] Audit log aggregation systems
- [ ] Verify log sanitization
- [ ] Review sensitive data in logs

**✅ Safe Practices:**
- Sanitize log messages
- Redact sensitive information
- Use structured logging

---

## 🔑 **Crypto / Key Material Sinks**

### ⚠️ **Critical Severity**
```javascript
// CRYPTO MISUSE
crypto.createCipher('aes-128-cbc', userPassword)
jwt.sign(payload, weakSecret)
crypto.randomBytes(4) // Weak entropy
```
**🔍 Analysis Checklist:**
- [ ] Review crypto algorithm selection
- [ ] Check for weak random number generation
- [ ] Audit key derivation functions
- [ ] Verify JWT secret strength
- [ ] Review cryptographic protocol usage

**✅ Safe Practices:**
- Use strong, standard algorithms
- Use cryptographically secure random generators
- Implement proper key management

---

## 🔌 **Third-Party SDKs/APIs That Evaluate Templates**

### ⚠️ **High Severity**
```javascript
// UNSAFE SDK USAGE
analytics.identify(userId, userTraits)
emailService.renderTemplate(userTemplate)
chartLibrary.render({ data: userData })
```
**🔍 Analysis Checklist:**
- [ ] Review third-party SDK initialization
- [ ] Check for template evaluation in SDKs
- [ ] Audit data passed to external services
- [ ] Verify SDK configuration security
- [ ] Review embedded widget configurations

**✅ Safe Practices:**
- Sandbox third-party content
- Validate data before sending to APIs
- Use Content Security Policy

---

## 💾 **Client-Side Storage Sinks**

### ⚠️ **Medium Severity**
```javascript
// UNSAFE STORAGE
localStorage.setItem('user', JSON.stringify(userData))
document.cookie = `session=${userToken}`
sessionStorage.setItem('data', sensitiveInfo)
```
**🔍 Analysis Checklist:**
- [ ] Review localStorage/sessionStorage usage
- [ ] Check cookie settings (HttpOnly, Secure)
- [ ] Audit sensitive data in client storage
- [ ] Verify storage data validation
- [ ] Review storage cleanup mechanisms

**✅ Safe Practices:**
- Avoid storing sensitive data client-side
- Use HttpOnly cookies for sessions
- Encrypt sensitive storage data

---

## 📄 **Client-Side File APIs**

### ⚠️ **Medium Severity**
```javascript
// FILE API RISKS
FileReader.readAsText(userFile)
URL.createObjectURL(maliciousFile)
new Blob([userContent], {type: 'text/html'})
```
**🔍 Analysis Checklist:**
- [ ] Review FileReader API usage
- [ ] Check object URL creation
- [ ] Audit file type validation
- [ ] Verify file content sanitization
- [ ] Review drag-and-drop handlers

**✅ Safe Practices:**
- Validate file types and sizes
- Sanitize file content when displayed
- Use CSP to restrict object URLs

---

## 🔐 **SSO / Token Handling Sinks**

### ⚠️ **Critical Severity**
```javascript
// TOKEN MISUSE
localStorage.setItem('jwt', token) // Insecure storage
jwt.verify(token, 'weak-secret')
oauth.callback(userRedirect)
```
**🔍 Analysis Checklist:**
- [ ] Review JWT storage and transmission
- [ ] Check token validation logic
- [ ] Audit OAuth/OIDC implementation
- [ ] Verify token expiration handling
- [ ] Review secret management

**✅ Safe Practices:**
- Use secure, HttpOnly cookies for tokens
- Validate token signatures properly
- Implement proper OAuth flows

---

## 🧩 **Misc: Dangerous Frameworks/Helpers**

### ⚠️ **Varies by Context**
```javascript
// FRAMEWORK-SPECIFIC RISKS
Vue.compile(userTemplate)
Angular.bypassSecurityTrustHtml(userContent)
React.createElement(userComponent)
```
**🔍 Analysis Checklist:**
- [ ] Review framework-specific safe/unsafe methods
- [ ] Check for security bypass functions
- [ ] Audit dynamic component rendering
- [ ] Verify framework security configurations
- [ ] Review custom directive implementations

**✅ Safe Practices:**
- Use framework security best practices
- Avoid security bypass functions
- Keep frameworks updated

---

## 🛡️ **General Security Analysis Tips**

### 🔍 **Code Review Process:**
1. **Trace Data Flow** - Follow user input from source to sink
2. **Context Awareness** - Understand the execution context
3. **Library Audit** - Review third-party dependencies
4. **Configuration Check** - Verify security settings
5. **Automated Scanning** - Use SAST tools (Semgrep, CodeQL)

### 🛠️ **Essential Tools:**
- **SAST**: Semgrep, SonarQube, CodeQL
- **Dependency Scanning**: `npm audit`, Snyk, OWASP DC
- **Runtime Protection**: CSP, Input validation libraries

### 📚 **Key Principles:**
- **Never trust user input**
- **Validate, then sanitize**
- **Use principle of least privilege**
- **Keep dependencies updated**
- **Implement defense in depth**

This checklist provides a structured approach to identifying and mitigating security risks in JavaScript applications. Use it as a guide during your code reviews to ensure comprehensive coverage.
