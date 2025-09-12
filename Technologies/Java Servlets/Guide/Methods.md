## 🔹 1. GET Example

### ✅ GET – Query Parameter

```java
@WebServlet("/api/user")
public class UserServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        // GET /api/user?city=Cairo
        // req. or request.
        String city = req.getParameter("city"); // query string param
        if (city == null) city = "Guest";

        resp.setContentType("application/json");
        resp.getWriter().println("{\"method\":\"GET\",\"city\":\"" + city + "\"}");
    }
}
```

### ✅ GET – Path Info (like Path Variable)

```java
@WebServlet("/api/user/*")
public class UserByIdServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        // GET /api/user/123
        // req. or request.
        String pathInfo = req.getPathInfo(); // "/123"
        int id = Integer.parseInt(pathInfo.substring(1));

        resp.setContentType("application/json");
        resp.getWriter().println("{\"method\":\"GET\",\"id\":" + id + "}");
    }
}
```

---

## 🔹 2. POST Example

### ✅ POST – JSON

```java
@WebServlet("/api/user/json")
public class PostJsonServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        // Read JSON from request body
        StringBuilder sb = new StringBuilder();
        try (BufferedReader reader = req.getReader()) {
            String line;
            while ((line = reader.readLine()) != null) sb.append(line);
        }
        String json = sb.toString();

        // (Normally use Jackson/Gson to parse JSON → Object)

        resp.setContentType("application/json");
        resp.getWriter().println("{\"method\":\"POST\",\"json\":\"" + json + "\"}");
    }
}
```

### ✅ POST – Form Data

```java
@WebServlet("/api/user/form")
public class PostFormServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        // POST form-data (application/x-www-form-urlencoded)
        // req. or request.
        String name = req.getParameter("name");

        resp.setContentType("application/json");
        resp.getWriter().println("{\"method\":\"POST\",\"name\":\"" + name + "\"}");
    }
}
```

---

## 🔹 3. PUT Example

```java
@WebServlet("/api/user/*")
public class PutUserServlet extends HttpServlet {
    @Override
    protected void doPut(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        // GET id from path
        // req. or request.
        String pathInfo = req.getPathInfo(); // "/5"
        int id = Integer.parseInt(pathInfo.substring(1));

        // Read JSON body
        String body = new BufferedReader(new InputStreamReader(req.getInputStream()))
                .lines().collect(java.util.stream.Collectors.joining());

        resp.setContentType("application/json");
        resp.getWriter().println("{\"method\":\"PUT\",\"id\":" + id + ",\"updatedUser\":\"" + body + "\"}");
    }
}
```

👉 Used for **full replacement** of a resource.

---

## 🔹 4. PATCH Example (Partial Update)

```java
@WebServlet("/api/user/*")
public class PatchUserServlet extends HttpServlet {
    @Override
    protected void doPatch(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        // PATCH requires HttpServlet 3.1+ or override service()
        // req. or request.
        String pathInfo = req.getPathInfo(); // "/7"
        int id = Integer.parseInt(pathInfo.substring(1));

        String body = new BufferedReader(new InputStreamReader(req.getInputStream()))
                .lines().collect(java.util.stream.Collectors.joining());

        resp.setContentType("application/json");
        resp.getWriter().println("{\"method\":\"PATCH\",\"id\":" + id + ",\"updatedFields\":\"" + body + "\"}");
    }
}
```

👉 ⚠️ Note: `doPatch()` isn’t standard before Servlet 3.1 — you may override `service()` and check the method name.

---

## 🔹 5. DELETE Example

```java
@WebServlet("/api/user/*")
public class DeleteUserServlet extends HttpServlet {
    @Override
    protected void doDelete(HttpServletRequest req, HttpServletResponse resp) throws IOException {
        // req. or request.
        String pathInfo = req.getPathInfo(); // "/10"
        int id = Integer.parseInt(pathInfo.substring(1));

        resp.setContentType("application/json");
        resp.getWriter().println("{\"method\":\"DELETE\",\"deletedUserId\":" + id + "}");
    }
}
```

---

## 🔹 6. File Uploads

### ✅ Single File

```java
@MultipartConfig
@WebServlet("/api/upload-single")
public class UploadSingleServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws IOException, ServletException {
        // req. or request.
        Part filePart = req.getPart("file"); // input name="file"
        String fileName = filePart.getSubmittedFileName();
        long size = filePart.getSize();

        resp.setContentType("application/json");
        resp.getWriter().println("{\"method\":\"POST\",\"filename\":\"" + fileName + "\",\"size\":" + size + "}");
    }
}
```

### ✅ Multiple Files

```java
@MultipartConfig
@WebServlet("/api/upload-multiple")
public class UploadMultipleServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws IOException, ServletException {
        // req. or request.
        Collection<Part> parts = req.getParts(); // input name="files"
        StringBuilder result = new StringBuilder("[");
        for (Part p : parts) {
            if (p.getName().equals("files")) {
                result.append("{\"name\":\"").append(p.getSubmittedFileName())
                      .append("\",\"size\":").append(p.getSize()).append("},");
            }
        }
        if (result.charAt(result.length()-1) == ',') result.deleteCharAt(result.length()-1);
        result.append("]");

        resp.setContentType("application/json");
        resp.getWriter().println("{\"method\":\"POST\",\"uploadedFiles\":" + result.toString() + "}");
    }
}
```

👉 **Important:** In HTML form →

```html
<form method="post" enctype="multipart/form-data" action="/api/upload-single">
    <input type="file" name="file"/>
    <button type="submit">Upload</button>
</form>
```

---

## 📝 Summary

✅ **Request Sources**

* 'req. or request.'
* `req.getParameter("name")` → query string or form data
* `req.getPathInfo()` → path variable (`/user/{id}`)
* `req.getReader()` / `req.getInputStream()` → JSON body
* `req.getPart("file")` / `req.getParts()` → single/multiple file uploads

✅ **HTTP Methods**

* GET → `doGet()`
* POST → `doPost()`
* PUT → `doPut()` (replace resource)
* PATCH → `doPatch()` (partial update – Servlet 3.1+)
* DELETE → `doDelete()`

✅ **Files**

* 'req. or request.'
* Single → `req.getPart("file")`
* Multiple → `req.getParts()`
