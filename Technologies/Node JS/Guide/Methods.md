# 📒 Node.js (Express) – Request Methods & File Uploads

---

## 🔹 1. GET Example

### ✅ GET – Query String

```js
// GET /user?city=Cairo
app.get("/user", (req, res) => {
    // req.query → query string params
    const city = req.query.city || "Guest";
    res.json({ method: "GET", type: "query", city });
});
```

### ✅ GET – Route Parameter

```js
// GET /user/123
app.get("/user/:id", (req, res) => {
    // req.params → route parameters
    const id = req.params.id;
    res.json({ method: "GET", type: "route", id });
});
```

---

## 🔹 2. POST Example

### ✅ POST – Form Data or JSON

```js
// POST /user
app.post("/user", (req, res) => {
    // req.body works for both JSON and URL-encoded form (with body-parser)
    const name = req.body.name || "Guest";
    res.json({ method: "POST", name });
});
```

---

## 🔹 3. PUT Example

```js
// PUT /user/123
app.put("/user/:id", (req, res) => {
    const id = req.params.id;
    const data = req.body; // JSON body
    res.json({ method: "PUT", id, updatedData: data });
});
```

---

## 🔹 4. PATCH Example

```js
// PATCH /user/123
app.patch("/user/:id", (req, res) => {
    const id = req.params.id;
    const partial = req.body; // JSON with fields to update
    res.json({ method: "PATCH", id, updatedFields: partial });
});
```

---

## 🔹 5. DELETE Example

```js
// DELETE /user/123
app.delete("/user/:id", (req, res) => {
    const id = req.params.id;
    res.json({ method: "DELETE", deletedId: id });
});
```

---

## 🔹 6. File Uploads

### ✅ Single File

```js
// POST /upload-single
app.post("/upload-single", upload.single("file"), (req, res) => {
    // req.file → single uploaded file
    const file = req.file;
    if (!file) return res.status(400).json({ error: "No file uploaded" });

    res.json({ method: "POST", filename: file.originalname, size: file.size });
});
```

### ✅ Multiple Files

```js
// POST /upload-multiple
app.post("/upload-multiple", upload.array("files", 10), (req, res) => {
    // req.files → array of uploaded files
    const files = req.files.map(f => ({ name: f.originalname, size: f.size }));
    res.json({ method: "POST", uploadedFiles: files });
});
```

# 📝 Summary

✅ **Request Sources**

* `req.query` → GET query string (`?key=value`)
* `req.params` → Route parameters (`/user/:id`)
* `req.body` → JSON or form-data (with body-parser)
* `req.file` → Single uploaded file
* `req.files` → Multiple uploaded files

✅ **Methods**

* `app.get()` → GET
* `app.post()` → POST
* `app.put()` → PUT
* `app.patch()` → PATCH
* `app.delete()` → DELETE

✅ **File Uploads**

* Single → `upload.single("file")`
* Multiple → `upload.array("files")`
