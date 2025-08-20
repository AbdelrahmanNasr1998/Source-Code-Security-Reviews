# 🌐 Ruby on Rails Project Structure & Routing

```
myapp/
│── app/
│   ├── controllers/
│   │   └── home_controller.rb      # Controllers: hold route actions
│   ├── models/
│   │   └── user.rb                 # Models (ActiveRecord)
│   └── views/
│       └── home/
│           └── index.html.erb      # Views (ERB templates)
│
├── config/
│   └── routes.rb                   # All routes defined here
│
├── db/
│   └── migrate/                    # Database migrations
│
└── public/                         # Public assets
```

---

## ⚙️ Routing Flow in Rails

1. **Entry Point**

```text
Browser URL → config/routes.rb → Controller → Action → View / Response
```

2. **Routes Definition (config/routes.rb)**

```ruby
Rails.application.routes.draw do
  # GET http://localhost:3000/
  root 'home#index'

  # GET http://localhost:3000/about
  get '/about', to: 'home#about'

  # POST http://localhost:3000/contact
  post '/contact', to: 'home#contact'

  # RESTful resource routes (users)
  # Generates index, show, new, create, edit, update, destroy
  resources :users
end
```

---

3. **Controller (app/controllers/home\_controller.rb)**

```ruby
class HomeController < ApplicationController
  # GET http://localhost:3000/
  def index
    # Renders app/views/home/index.html.erb by default
  end

  # GET http://localhost:3000/about
  def about
    render plain: "About Page"
  end

  # POST http://localhost:3000/contact
  def contact
    name = params[:name]
    render plain: "Contact submitted: #{name}"
  end
end
```

---

4. **Route Parameters & Query Parameters**

```ruby
# GET http://localhost:3000/users/5
get '/users/:id', to: 'users#show'

# Controller action
def show
  id = params[:id]
  render plain: "User ID: #{id}"
end

# GET http://localhost:3000/search?name=Alice
get '/search', to: 'users#search'

def search
  name = params[:name]
  render plain: "Search: #{name}"
end
```

---

5. **RESTful Resource Example (app/controllers/users\_controller.rb)**

```ruby
class UsersController < ApplicationController
  # GET http://localhost:3000/users
  def index
    render json: User.all
  end

  # GET http://localhost:3000/users/5
  def show
    render json: User.find(params[:id])
  end

  # POST http://localhost:3000/users
  def create
    user = User.create(params.require(:user).permit(:name, :email))
    render json: user
  end
end
```

---

## 🔄 Flow Summary

`Browser URL → config/routes.rb → Controller#Action → (optional) View (.html.erb) or JSON`

---

## 🔑 Special Rails Routing Notes

* **Conventions over configuration**:

  * `Controller#action` naming is conventional
  * `render` automatically looks in `app/views/{controller}/{action}.html.erb`
* **RESTful routing**:

  * `resources :users` creates 7 default routes: `index, show, new, create, edit, update, destroy`
* **Route helpers**:

  * `root_path` → `/`
  * `users_path` → `/users`
  * `user_path(id)` → `/users/:id`
* **Route types**:

  * `get`, `post`, `patch`, `put`, `delete`
* **Query parameters**: Available in `params` hash
