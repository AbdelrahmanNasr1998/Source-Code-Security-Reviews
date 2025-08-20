# ⚡ Express.js – Controller & Routes in Same File

```
myapp/
│── app.js
│── controllers/
│   └── homeController.js
```

### **🖥️ app.js** 

```js
const express = require('express');
const app = express();
const homeRoutes = require('./controllers/homeController');

// Use homeController routes
app.use('/', homeRoutes);

app.listen(3000, () => {
  console.log('Server running on http://localhost:3000');
});
```

### **📂 controllers/homeController.js** 

```js
const express = require('express');
const router = express.Router();

// Define routes + handlers here directly
// GET http://localhost:3000/
router.get('/', (req, res) => {
  res.send('Hello from Home Page');
});

// GET http://localhost:3000/about
router.get('/about', (req, res) => {
  res.send('About Page');
});

module.exports = router;
```

**Flow:**
`🌐 Browser → 🖥️ app.js → 📂 homeController.js (router + function) → 📨 response`

---

# 🌐 Express.js – Multiple Routers in Controllers

```
myapp/
│── app.js
│── controllers/
│   └── userController.js
│   └── productController.js
```

### **🖥️ app.js** 

```js
const express = require('express');
const app = express();

// Mount routers
const userRoutes = require('./controllers/userController');
const productRoutes = require('./controllers/productController');

app.use('/users', userRoutes);
app.use('/products', productRoutes);

app.listen(3000, () => {
  console.log('Server running on http://localhost:3000');
});
```

### **📂 controllers/userController.js** 

```js
const express = require('express');
const router = express.Router();

// GET http://localhost:3000/users/
router.get('/', (req, res) => {
  res.json([{ id: 1, name: 'Alice' }]);
});

// POST http://localhost:3000/users/
router.post('/', (req, res) => {
  res.json({ message: 'User created' });
});

module.exports = router;
```

### **📂 controllers/productController.js** 

```js
const express = require('express');
const router = express.Router();

// GET http://localhost:3000/products/
router.get('/', (req, res) => {
  res.json([{ id: 10, product: 'Laptop' }]);
});

// POST http://localhost:3000/products/
router.post('/', (req, res) => {
  res.json({ message: 'Product added' });
});

module.exports = router;
```

**Flow:**
`🌐 Browser → 🖥️ app.js → /users → 📂 userController.js router → 📨 response`
`🌐 Browser → 🖥️ app.js → /products → 📂 productController.js router → 📨 response`

---

# 📝 Bare Express.js – All in `app.js` (Tiny Apps)

```
myapp/
│── app.js
```

### **🖥️ app.js** 

```js
const express = require('express');
const app = express();

// Routes directly in app.js
// GET http://localhost:3000/
app.get('/', (req, res) => res.send('Home Page'));

// GET http://localhost:3000/about
app.get('/about', (req, res) => res.send('About Page'));

// GET http://localhost:3000/contact
app.get('/contact', (req, res) => res.send('Contact Page'));

app.listen(3000, () => console.log('Server running on http://localhost:3000'));
```

**Flow:**
`🌐 Browser → 🖥️ app.js (routes inline) → 📨 response`

---

# 📌 Summary of URLs

* **🧩 Single Controller Router** 

  * `/` → `homeController.js → router.get('/')`
  * `/about` → `homeController.js → router.get('/about')`

* **🗂️ Multiple Routers** 

  * `/users/` → `userController.js → router.get('/')`
  * `/users/ (POST)` → `userController.js → router.post('/')`
  * `/products/` → `productController.js → router.get('/')`
  * `/products/ (POST)` → `productController.js → router.post('/')`

* **🖥️ Inline Routes in app.js**

  * `/` → `app.get('/')`
  * `/about` → `app.get('/about')`
  * `/contact` → `app.get('/contact')`

---

# ⚙️ Routing Options in Node.js

* **Bare Node.js (http module):** `if (req.url === ...) { ... }`
* **Express Inline:** Routes inside `app.js`
* **Express Router (Separate Files):** `routes/ + controllers/`
* **Router in Same Controller:** Define `router.get(...)` directly inside controller file
* **Modular REST APIs:** Multiple routers (e.g., `userRoutes`, `productRoutes`)
