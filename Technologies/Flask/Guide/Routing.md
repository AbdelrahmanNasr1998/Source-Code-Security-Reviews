# 🌐 Flask Project Structure & Routing

```
myapp/
│── app.py                 # Main app and routes
│── templates/
│   └── index.html         # HTML templates
│── static/
│   └── style.css          # Static files (CSS, JS, images)
│── models.py              # Optional: database models
```

---

## ⚙️ Routing Flow in Flask

1. **Main App (app.py)**

```python
from flask import Flask, render_template, request, jsonify

app = Flask(__name__)

# GET http://localhost:5000/
@app.route('/')
def index():
    return render_template('index.html')  # Renders templates/index.html

# GET http://localhost:5000/about
@app.route('/about')
def about():
    return "About Page"

# POST http://localhost:5000/contact
@app.route('/contact', methods=['POST'])
def contact():
    name = request.form.get('name')
    return f"Contact submitted: {name}"
```

---

2. **Route Parameters & Query Parameters**

```python
# GET http://localhost:5000/users/5
@app.route('/users/<int:id>')
def get_user(id):
    return f"User ID: {id}"

# GET http://localhost:5000/search?name=Alice
@app.route('/search')
def search():
    name = request.args.get('name')
    return f"Search: {name}"
```

---

3. **REST API Example**

```python
from flask import Flask, jsonify, request

app = Flask(__name__)

users = [{"id": 1, "name": "Alice"}]

# GET http://localhost:5000/api/users
@app.route('/api/users', methods=['GET'])
def get_users():
    return jsonify(users)

# POST http://localhost:5000/api/users
@app.route('/api/users', methods=['POST'])
def create_user():
    data = request.json
    users.append(data)
    return jsonify(data), 201

# GET http://localhost:5000/api/users/1
@app.route('/api/users/<int:id>', methods=['GET'])
def get_user_by_id(id):
    user = next((u for u in users if u['id'] == id), None)
    return jsonify(user) if user else ("Not found", 404)
```

---

## 🔄 Flow Summary

`Browser URL → app.py → @app.route decorated function → (optional) template / JSON / text response`

---

## 🔑 Special Flask Routing Notes

* **Route decorator**: `@app.route('/path', methods=['GET', 'POST', ...])`
* **Dynamic URL parameters**: `<int:id>`, `<string:name>`, `<path:subpath>`
* **Query parameters**: `request.args.get('param')`
* **Request body**: `request.form` for form data, `request.json` for JSON
* **Templates**: `render_template('template.html', **context)` → renders HTML
* **Blueprints**: For larger apps, routes can be organized in modules via `Blueprint`s
If you want, I can **now make a complete “mega routing reference”** showing **all frameworks side by side**, with **full URLs, route decorators/annotations, controller functions, and responses**.
Do you want me to do that?
