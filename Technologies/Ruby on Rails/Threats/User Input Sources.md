# 💎 **Ruby on Rails – User Input Sources**

---

## 🔹 1. Query String (GET parameters)

```ruby
# Example: GET /search?name=admin
name = params[:name]
```

---

## 🔹 2. Route Parameters (Path Variables)

```ruby
# config/routes.rb → get "/users/:id" => "users#show"
# Example: GET /users/42
def show
  user_id = params[:id]  # "42"
end
```

---

## 🔹 3. Form Data (POST parameters)

```ruby
# Example: POST /login with form fields
def login
  username = params[:username]
  password = params[:password]
end
```

---

## 🔹 4. JSON Body

```ruby
# Example: POST /api with JSON: { "email": "test@example.com" }
def create
  email = params[:email]   # Rails automatically parses JSON into params
end
```

---

## 🔹 5. File Uploads

```ruby
# Example: POST /upload with file input
def upload
  file = params[:avatar]   # ActionDispatch::Http::UploadedFile
  file.original_filename
  file.read
end

# Multiple files
def upload_multi
  params[:photos].each do |photo|
    puts photo.original_filename
  end
end
```

---

## 🔹 6. Headers

```ruby
user_agent = request.headers["User-Agent"]
ip = request.headers["X-Forwarded-For"]
```

---

## 🔹 7. Cookies

```ruby
# Read
session_id = cookies[:session_id]

# Write
cookies[:theme] = "dark"
```

---

## 🔹 8. Session Data

```ruby
session[:user_id] = 42
current_user_id = session[:user_id]
```

---

## 🔹 9. Raw Request Body

```ruby
raw_body = request.raw_post
```

---

## 🔹 10. All Inputs at Once

```ruby
# params contains GET, POST, JSON, Route params
all_inputs = params.to_unsafe_h
```
