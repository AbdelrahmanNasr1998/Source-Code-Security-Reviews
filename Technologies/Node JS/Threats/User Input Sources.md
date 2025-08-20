# 🌐 **Node.js – User Input Sources**

---

## 🔹 1. Query String (GET parameters)

**Native Node.js (http):**

```js
// Example: GET /search?name=admin
const url = require('url');
const queryObject = url.parse(req.url, true).query;

const name = queryObject.name;
```

**Express.js:**

```js
// Example: GET /search?name=admin
const name = req.query.name;
```

---

## 🔹 2. Route Parameters (Path Variables)

**Native Node.js (http):**

```js
// No built-in routing → must parse URL manually
const parts = req.url.split('/');
const id = parts[2];  // Example: /user/42 → id = "42"
```

**Express.js:**

```js
app.get('/user/:id', (req, res) => {
    const id = req.params.id;
    res.send("User ID = " + id);
});
```

---

## 🔹 3. Form Data (POST parameters)

**Native Node.js (http):**

```js
let body = '';
req.on('data', chunk => { body += chunk; });
req.on('end', () => {
    const params = new URLSearchParams(body);
    const username = params.get('username');
});
```

**Express.js (with body-parser / built-in middleware):**

```js
app.post('/login', (req, res) => {
    const username = req.body.username;
    const password = req.body.password;
});
```

---

## 🔹 4. JSON Body

**Native Node.js (http):**

```js
let raw = '';
req.on('data', chunk => { raw += chunk; });
req.on('end', () => {
    const data = JSON.parse(raw);
    const email = data.email;
});
```

**Express.js:**

```js
app.post('/api', (req, res) => {
    const email = req.body.email;  // JSON parsed automatically
});
```

---

## 🔹 5. File Uploads

**Native Node.js (http):**

```js
// Must parse multipart manually or use libraries like `formidable` / `multer`
```

**Express.js (with Multer):**

```js
// Single file
app.post('/upload', upload.single('avatar'), (req, res) => {
    console.log(req.file.originalname);
});

// Multiple files
app.post('/upload-multi', upload.array('photos'), (req, res) => {
    req.files.forEach(file => console.log(file.originalname));
});
```

---

## 🔹 6. Headers

**Native Node.js (http):**

```js
const ua = req.headers['user-agent'];
const ip = req.headers['x-forwarded-for'];
```

**Express.js:**

```js
const ua = req.get('User-Agent');
const ip = req.get('X-Forwarded-For');
```

---

## 🔹 7. Cookies

**Native Node.js (http):**

```js
const cookieHeader = req.headers['cookie'];
// Example: "session_id=12345; theme=dark"
```

**Express.js (with cookie-parser):**

```js
const sessionId = req.cookies['session_id'];
```

---

## 🔹 8. Session Data

**Native Node.js:**
(no built-in sessions, must implement or use libraries)

**Express.js (with express-session):**

```js
app.get('/profile', (req, res) => {
    const user = req.session.user;
});
```

---

## 🔹 9. Raw Request Body

**Native Node.js (http):**

```js
let raw = '';
req.on('data', chunk => { raw += chunk; });
req.on('end', () => {
    console.log(raw);
});
```

**Express.js:**

```js
const raw = req.body;  // when using raw middleware
```

---

## 🔹 10. All Inputs at Once

**Native Node.js (http):**

```js
// No built-in equivalent, must merge query + body + cookies manually
```

**Express.js:**

```js
const query = req.query;   // GET
const body = req.body;     // POST/JSON
const params = req.params; // Route params
```
