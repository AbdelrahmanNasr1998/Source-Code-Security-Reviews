# 🌐 Laravel Project Structure & Routing

```
myapp/
│── app/
│   ├── Http/
│   │   ├── Controllers/
│   │   │   └── HomeController.php    # Controllers: hold route logic
│   │   └── Middleware/               # Optional middleware for routes
│   ├── Models/
│   │   └── User.php                  # Eloquent models (DB)
│
├── routes/
│   ├── web.php    # Routes for web pages (HTML)
│   └── api.php    # Routes for API (JSON)
│
├── resources/
│   └── views/
│       └── welcome.blade.php         # Blade templates
│
└── public/
    └── index.php                      # Entry point for all requests
```

---

## ⚙️ Routing Flow in Laravel

1. **Entry Point**

```text
Browser URL → public/index.php → routes/web.php (or api.php) → Controller → View/Response
```

---

2. **Web Routes (routes/web.php)**

```php
<?php
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\HomeController;

// GET http://localhost:8000/
Route::get('/', [HomeController::class, 'index']);

// GET http://localhost:8000/about
Route::get('/about', [HomeController::class, 'about']);

// POST http://localhost:8000/contact
Route::post('/contact', [HomeController::class, 'contact']);
```

---

3. **Controller (app/Http/Controllers/HomeController.php)**

```php
<?php
namespace App\Http\Controllers;

use Illuminate\Http\Request;

class HomeController extends Controller
{
    // GET http://localhost:8000/
    public function index() {
        return view('welcome'); // resources/views/welcome.blade.php
    }

    // GET http://localhost:8000/about
    public function about() {
        return "About Page";
    }

    // POST http://localhost:8000/contact
    public function contact(Request $request) {
        $name = $request->input('name');
        return "Contact submitted: $name";
    }
}
```

---

4. **Route Parameters & Query Parameters**

```php
// GET http://localhost:8000/users/5
Route::get('/users/{id}', [HomeController::class, 'userById']);

// Controller
public function userById($id) {
    return "User ID: $id";
}

// GET http://localhost:8000/search?name=Alice
Route::get('/search', [HomeController::class, 'search']);

public function search(Request $request) {
    $name = $request->query('name');
    return "Search: $name";
}
```

---

5. **API Routes (routes/api.php)**

```php
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\Api\UserController;

// Base URL for API: http://localhost:8000/api
Route::get('/users', [UserController::class, 'index']);      // GET http://localhost:8000/api/users
Route::post('/users', [UserController::class, 'store']);     // POST http://localhost:8000/api/users
Route::get('/users/{id}', [UserController::class, 'show']);  // GET http://localhost:8000/api/users/5
```

---

## 🔄 Flow Summary

* **Web Requests**:
  `Browser → public/index.php → routes/web.php → Controller → Blade View / Response`

* **API Requests**:
  `HTTP Client → public/index.php → routes/api.php → Controller → JSON Response`

---

## 🔑 Special Laravel Routing Notes

* **Route Types**: `get`, `post`, `put`, `delete`, `patch`, `options`
* **Route Parameters**: `{id}`, `{slug}`, optional `{id?}`
* **Route Middleware**: `->middleware('auth')` protects routes
* **Route Grouping & Prefix**:

```php
Route::prefix('admin')->group(function () {
    Route::get('/dashboard', [AdminController::class, 'dashboard']);
});
// GET http://localhost:8000/admin/dashboard
```

* **Controller Routing Syntax**:

```php
Route::get('/path', [ControllerName::class, 'methodName']);
```

* **API Base URL**: All API routes are prefixed with `/api` automatically
