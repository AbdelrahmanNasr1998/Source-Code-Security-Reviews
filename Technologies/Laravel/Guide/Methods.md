## 🔹 1. Request Methods

Laravel routes support different HTTP methods:

```php
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\MyController;

Route::get('/hello', [MyController::class, 'getHello']);       // GET
Route::post('/hello', [MyController::class, 'postHello']);     // POST
Route::put('/hello', [MyController::class, 'putHello']);       // PUT
Route::delete('/hello', [MyController::class, 'deleteHello']); // DELETE
Route::match(['get','post'], '/match', [MyController::class, 'matchMethod']); // GET or POST
Route::any('/any', [MyController::class, 'anyMethod']);        // Any method
```

### Controller Example

```php
namespace App\Http\Controllers;

use Illuminate\Http\Request;

class MyController extends Controller
{
    public function getHello(Request $request) { return "GET request"; }
    public function postHello(Request $request) { return "POST request"; }
    public function putHello(Request $request) { return "PUT request"; }
    public function deleteHello(Request $request) { return "DELETE request"; }
}
```

---

## 🔹 2. Receiving GET Data

### a) Via Query String

```php
// Example URL: /items?id=5&name=Phone
public function getItem(Request $request)
{
    $id = $request->query('id');        // or $request->input('id')
    $name = $request->query('name');    
    return "GET query: id=$id, name=$name";
}
```

### b) Via Route Parameter

```php
// Route: /items/{id}
Route::get('/items/{id}', [MyController::class, 'getItemById']);

public function getItemById($id)
{
    return "GET route param: id=$id";
}
```

---

## 🔹 3. Receiving Data from GET & POST

### a) Form Data (GET/POST)

```php
// POST /items/form → form-data: name=Book, price=20
public function postItemForm(Request $request)
{
    $name = $request->input('name');   // works for GET or POST form
    $price = $request->input('price');
    return "Form data: $name, price=$price";
}
```

### b) JSON Body (POST/PUT)

```php
// POST /items/json → JSON: {"name":"Laptop","price":1000}
public function postItemJson(Request $request)
{
    $name = $request->json('name');
    $price = $request->json('price');
    return "JSON data: $name, price=$price";
}
```

---

## 🔹 4. File Uploads

### a) Single File

```php
public function uploadSingle(Request $request)
{
    if ($request->hasFile('file')) {
        $file = $request->file('file');
        $filename = $file->getClientOriginalName();
        $size = $file->getSize();
        return "Uploaded file: $filename, size: $size bytes";
    }
    return "No file uploaded";
}
```

### b) Multiple Files

```php
public function uploadMultiple(Request $request)
{
    if ($request->hasFile('files')) {
        $files = $request->file('files');
        $response = "Uploaded files: ";
        foreach ($files as $file) {
            $response .= $file->getClientOriginalName() . " (" . $file->getSize() . " bytes), ";
        }
        return $response;
    }
    return "No files uploaded";
}
```

### HTML Form Example

```html
<form action="/upload/single" method="post" enctype="multipart/form-data">
    @csrf
    <input type="file" name="file">
    <button type="submit">Upload Single</button>
</form>

<form action="/upload/multiple" method="post" enctype="multipart/form-data">
    @csrf
    <input type="file" name="files[]" multiple>
    <button type="submit">Upload Multiple</button>
</form>
```
