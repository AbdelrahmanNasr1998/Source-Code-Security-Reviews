## 🔹 1. Request Methods in Routes

Flask uses the `methods` argument in the route decorator:

```python
from flask import Flask, request

app = Flask(__name__)

@app.route("/hello", methods=["GET"])
def hello_get():
    return "GET request"

@app.route("/hello", methods=["POST"])
def hello_post():
    return "POST request"

@app.route("/hello", methods=["PUT"])
def hello_put():
    return "PUT request"

@app.route("/hello", methods=["DELETE"])
def hello_delete():
    return "DELETE request"
```

---

## 🔹 2. Receiving Data

### a) Query String (GET)

```python
# Example URL: /items?id=5&name=Phone
@app.route("/items", methods=["GET"])
def get_item():
    id = request.args.get("id")
    name = request.args.get("name")
    return f"GET query: id={id}, name={name}"
```

### b) Route Parameter

```python
# Route: /items/<int:id>
@app.route("/items/<int:id>", methods=["GET"])
def get_item_by_id(id):
    return f"GET route param: id={id}"
```

---

### c) Form Data (POST)

```python
# POST form-data: name=Book, price=20
@app.route("/items/form", methods=["POST"])
def post_item_form():
    name = request.form.get("name")
    price = request.form.get("price")
    return f"Form data: {name}, price={price}"
```

### d) JSON Body (POST/PUT)

```python
# POST JSON: {"name": "Laptop", "price": 1000}
@app.route("/items/json", methods=["POST"])
def post_item_json():
    data = request.get_json()
    name = data.get("name")
    price = data.get("price")
    return f"JSON data: {name}, price={price}"
```

> ⚡ Note: `request.args` = query string, `request.form` = form data, `request.get_json()` = JSON body.

---

## 🔹 3. File Uploads

Flask provides `request.files` for uploaded files.
Make sure HTML form uses `enctype="multipart/form-data"`.

### a) Single File Upload

```python
@app.route("/upload/single", methods=["POST"])
def upload_single():
    if "file" in request.files:
        file = request.files["file"]
        filename = file.filename
        size = len(file.read())
        file.seek(0)  # reset pointer if needed
        return f"Uploaded file: {filename}, size: {size} bytes"
    return "No file uploaded"
```

### b) Multiple Files Upload

```python
@app.route("/upload/multiple", methods=["POST"])
def upload_multiple():
    if "files" in request.files:
        files = request.files.getlist("files")
        response = "Uploaded files: "
        for file in files:
            size = len(file.read())
            file.seek(0)
            response += f"{file.filename} ({size} bytes), "
        return response
    return "No files uploaded"
```

### HTML Form Example

```html
<!-- Single file -->
<form action="/upload/single" method="post" enctype="multipart/form-data">
    <input type="file" name="file">
    <button type="submit">Upload Single</button>
</form>

<!-- Multiple files -->
<form action="/upload/multiple" method="post" enctype="multipart/form-data">
    <input type="file" name="files" multiple>
    <button type="submit">Upload Multiple</button>
</form>
```
