## 🔹 1. Request Methods in Routes

Rails uses the `routes.rb` file to define HTTP methods:

```ruby
# config/routes.rb
Rails.application.routes.draw do
  get '/hello', to: 'my#hello_get'          # GET
  post '/hello', to: 'my#hello_post'       # POST
  put '/hello', to: 'my#hello_put'         # PUT
  patch '/hello', to: 'my#hello_patch'     # PATCH
  delete '/hello', to: 'my#hello_delete'   # DELETE
end
```

### Controller Example

```ruby
# app/controllers/my_controller.rb
class MyController < ApplicationController
  def hello_get
    render plain: "GET request"
  end

  def hello_post
    render plain: "POST request"
  end

  def hello_put
    render plain: "PUT request"
  end

  def hello_patch
    render plain: "PATCH request"
  end

  def hello_delete
    render plain: "DELETE request"
  end
end
```

---

## 🔹 2. Receiving Data

### a) Query String (GET)

```ruby
# GET /items?id=5&name=Phone
def get_item
  id = params[:id]
  name = params[:name]
  render plain: "GET query: id=#{id}, name=#{name}"
end
```

### b) Route Parameter

```ruby
# Route: /items/:id
get '/items/:id', to: 'my#get_item_by_id'

def get_item_by_id
  id = params[:id]
  render plain: "GET route param: id=#{id}"
end
```

---

### c) Form Data (POST)

```ruby
# POST /items/form → form-data: name=Book, price=20
def post_item_form
  name = params[:name]
  price = params[:price]
  render plain: "Form data: #{name}, price=#{price}"
end
```

### d) JSON Body (POST/PUT)

```ruby
# POST /items/json → JSON: {"name":"Laptop","price":1000}
def post_item_json
  name = params[:name]   # Rails automatically parses JSON into params
  price = params[:price]
  render plain: "JSON data: #{name}, price=#{price}"
end
```

> ⚡ Note: Rails automatically parses both form and JSON bodies into `params`.

---

## 🔹 3. File Uploads

Rails provides `params[:file]` for single file and `params[:files]` for multiple files.
Make sure the HTML form uses `multipart/form-data`.

### a) Single File Upload

```ruby
def upload_single
  if params[:file].present?
    file = params[:file]
    filename = file.original_filename
    size = file.size
    render plain: "Uploaded file: #{filename}, size: #{size} bytes"
  else
    render plain: "No file uploaded"
  end
end
```

### b) Multiple Files Upload

```ruby
def upload_multiple
  if params[:files].present?
    files = params[:files]
    response = "Uploaded files: "
    files.each do |file|
      response += "#{file.original_filename} (#{file.size} bytes), "
    end
    render plain: response
  else
    render plain: "No files uploaded"
  end
end
```

### HTML Form Example

```erb
<!-- Single file -->
<%= form_with url: '/upload/single', local: true, multipart: true do |f| %>
  <%= f.file_field :file %>
  <%= f.submit "Upload Single" %>
<% end %>

<!-- Multiple files -->
<%= form_with url: '/upload/multiple', local: true, multipart: true do |f| %>
  <%= f.file_field :files, multiple: true %>
  <%= f.submit "Upload Multiple" %>
<% end %>
```
