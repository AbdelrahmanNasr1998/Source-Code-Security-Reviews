## 🔹 1. Function-Based Views (FBVs)

### ✅ GET (Query Params)

```python
from django.http import JsonResponse
from django.urls import path

# GET via query string: /profile/?user=1
def get_example_by_query(request):
    # request.GET → fetch from query string
    user_id = request.GET.get("user", None)
    return JsonResponse({"method": "GET", "source": "query", "userId": user_id})

# GET via route parameter: /profile/user/1
def get_example_by_route(request, user_id):
    # user_id comes from URL captured in urls.py
    return JsonResponse({"method": "GET", "source": "route", "userId": user_id})
```

---

### ✅ POST (Form & JSON)

```python
import json
from django.views.decorators.csrf import csrf_exempt
from django.http import JsonResponse

@csrf_exempt  # disable CSRF check (useful for testing APIs, but not in production)
def post_example(request):
    if request.method == "POST":
        if request.content_type == "application/json":
            # request.body contains raw JSON bytes → must be parsed
            data = json.loads(request.body)
            name = data.get("name", "Guest")
        else:
            # request.POST works only for form-data (not JSON)
            name = request.POST.get("name", "Guest")
        return JsonResponse({"method": "POST", "name": name})
    return JsonResponse({"error": "POST only"}, status=405)
```

---

### ✅ PUT

```python
@csrf_exempt
def put_example(request):
    if request.method == "PUT":
        # PUT usually replaces an entire resource → body must be JSON
        data = json.loads(request.body)
        return JsonResponse({"method": "PUT", "data": data})
    return JsonResponse({"error": "PUT only"}, status=405)
```

---

### ✅ PATCH (Partial Update)

```python
@csrf_exempt
def patch_example(request):
    if request.method == "PATCH":
        # PATCH is for partial updates → update only given fields
        data = json.loads(request.body)
        return JsonResponse({"method": "PATCH", "updated_fields": data})
    return JsonResponse({"error": "PATCH only"}, status=405)
```

---

### ✅ DELETE

```python
@csrf_exempt
def delete_example(request):
    if request.method == "DELETE":
        # Some clients send data in DELETE, some don’t → handle both
        data = json.loads(request.body or "{}")
        return JsonResponse({"method": "DELETE", "deleted": data})
    return JsonResponse({"error": "DELETE only"}, status=405)
```

---

### ✅ File Upload (Single + Multiple)

```python
@csrf_exempt
def upload_view(request):
    if request.method == "POST":
        # Single file (input name="file")
        one_file = request.FILES.get("file")  

        # Multiple files (input name="files" multiple)
        many_files = request.FILES.getlist("files")  

        return JsonResponse({
            "single_file": one_file.name if one_file else None,
            "multi_files": [f.name for f in many_files]
        })
    return JsonResponse({"error": "POST only"}, status=405)
```

👉 **Important**: The HTML form must include

```html
<form enctype="multipart/form-data"> ... </form>
```

---

## 🔹 2. Class-Based Views (CBVs)

```python
from django.views import View
from django.http import JsonResponse
from django.utils.decorators import method_decorator
from django.views.decorators.csrf import csrf_exempt
import json

@method_decorator(csrf_exempt, name="dispatch")  # Apply csrf_exempt to the whole class
class CRUDView(View):
    def get(self, request):
        # GET params
        return JsonResponse({"method": "GET", "city": request.GET.get("city", "Guest")})

    def post(self, request):
        if request.content_type == "application/json":
            data = json.loads(request.body)
            user = data.get("user", "Guest")
        else:
            user = request.POST.get("user", "Guest")
        return JsonResponse({"method": "POST", "user": user})

    def put(self, request):
        data = json.loads(request.body)
        return JsonResponse({"method": "PUT", "data": data})

    def patch(self, request):
        data = json.loads(request.body)
        return JsonResponse({"method": "PATCH", "updated": data})

    def delete(self, request):
        data = json.loads(request.body or "{}")
        return JsonResponse({"method": "DELETE", "deleted": data})
```

---

## 🔹 3. Django REST Framework (DRF)

### ✅ Full CRUD Example

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.parsers import MultiPartParser, FormParser, JSONParser

class CRUDAPI(APIView):
    # Important: Parsers define how DRF understands the request body
    parser_classes = [MultiPartParser, FormParser, JSONParser]

    def get(self, request):
        # DRF uses request.query_params instead of request.GET
        return Response({"method": "GET", "city": request.query_params.get("city", "Guest")})

    def post(self, request):
        # request.data works for form-data, JSON, and multipart
        user = request.data.get("user", "Guest")

        # Files come from request.FILES
        file = request.FILES.get("file")          # Single
        files = request.FILES.getlist("files")    # Multiple

        return Response({
            "method": "POST",
            "user": user,
            "single_file": file.name if file else None,
            "multi_files": [f.name for f in files]
        })

    def put(self, request):
        # Replace the resource with new data
        return Response({"method": "PUT", "data": request.data})

    def patch(self, request):
        # Update only some fields
        return Response({"method": "PATCH", "updated_fields": request.data})

    def delete(self, request):
        # DRF also allows request.data in DELETE (if client sends JSON)
        return Response({"method": "DELETE", "data": request.data})
```

---

# 📝 Summary (with Key Notes)

✅ **FBV/CBV**

* `request.GET` → Query params (`?key=value`)
* `request.POST` → Form fields only
* `json.loads(request.body)` → For raw JSON (POST/PUT/PATCH/DELETE)
* `request.FILES.get("file")` → One file
* `request.FILES.getlist("files")` → Multiple files

✅ **DRF**

* `request.query_params` → GET params
* `request.data` → JSON / form-data / multipart
* `request.FILES` → File objects
* `parser_classes` → Needed for file & JSON parsing

---

