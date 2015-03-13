# Let's Build a Web App

## Set Up Nitruous.io

Follow screenshots in Keynote.

## Basic Unix Commands

```
> pwd

> ls -la

> ruby -v

> rails -v

```

## Create a Rails App

Let's get started.

```
> cd workspace

> rails new hobbies

> cd hobbies

> ls
```

Start the server.

```
> rails server
```

Create a new Home page.

```
> rails generate controller welcome index
```

Modify `welcome/index.html.erb`

```
  <h1>Hello World</h1>
  <p>This is my hobbies site</p>
```

Modify `routes.rb`

```
  root 'welcome#index'
```

Browse to `http://localhost/` and you'll see your new Home page.

----

### Routes and Controllers

__We want to build a form that we can use to store a Hobby in the `Database`.__

> Explain: What is `Database`? Why do we need to store a Hobby in the `Database`?


Let's start with the `Routes`.

```
  resources :hobbies
```

Browse to `/rails/info/routes` 

> Explain: What are `Routes`? What `Routes` were created?

Browse to `/hobbies/new` => uninitialized controller!

```
> rails generate controller hobbies
```

Browse to `/hobbies/new` => action `new` cannot be found!

```
  class HobbiesController < ApplicationController
    def new
      @hobby = Hobby.new
    end
  end
```

Browse to `/hobbies/new` => template missing!

Create an empty file at `/app/views/hobbies/new.html`.

```
  <h1>New Hobby</h1>
  <%= form_for @hobby do |f| %>
    <p>
      <%= f.label :name %><br>
      <%= f.text_field :name %>
    </p> 
    <p>
      <%= f.submit %>
    </p>
  <% end %>
```

Browse to `/hobbies/new` and now you see a form! Yea!

Let's try and submit => action `create` canot be found!

```
  class HobbiesController < ApplicationController
    def new
    end
    
    def create
      render plain: params[:hobby].inspect
    end
  end
```

---

### Models

__We want to build a form that we can use to store a Hobby in the `Database`.__

> Explain: What is `Database`? Why do we need to store a Hobby in the `Database`?

Now we need to create the `Model` that will store our Hobby.

```
> rails generate model Hobby name:string
```

Open up the migration file `*_create_hobbies.rb`

```
  class CreateHobbies < ActiveRecord::Migration
    def change
      create_table :hobbies do |t|
        t.string :name		

        t.timestamps
      end
    end
  end
```

Run the migration.

> Explain: What is `Migration`?

```
> rake db:migrate
```

To save our Hobby in the `Database`, modify the `create` method in `hobbies_controller.rb`.

```
  class HobbiesController < ApplicationController
    def new
    end
    
    def create
      @hobby = Hobby.new(params.require(:hobby).permit(:name))
      @hobby.save
      redirect_to @hobby
    end
  end
```

Then, we add the `show` method.

```
  class HobbiesController < ApplicationController
    ...
    def show
      @hobby = Hobby.find(params[:id])
    end
    ...
  end
```

Create `/app/views/hobbies/show.html.erb`.

```
  <p>
    <strong>Name:</strong>
    <%= @hobby.name %>
  </p>
```

Now you can create a new Hobby at `/hobbies/new`. Yea!

---

### List All Hobbies

Modify the `index` method.

```
  class HobbiesController < ApplicationController
    ...
    def index
      @hobbies = Hobby.all
    end
    ...
  end
```

Add the view at `app/views/hobbies/index.html.erb`.

```
  <h1>My Hobbies</h1>
  <% @hobbies.each_with_index do |hobby, index| %>
    <p>Hobby <%= index %>: <%= hobby.name %></p>
  <% end %>
  <%= link_to new_hobbies_path %>
```

---

### Edit a Hobby

Add `edit` method.

```
  class HobbiesController < ApplicationController
    ...
    def edit
      @hobby = Hobby.find(params[:id])
    end
    ...
  end
```

Copy `app/views/hobbies/new.html.erb` to `app/views/hobbies/edit.htnml.erb`.

```
  <h1>Edit Hobby</h1>
  <%= form_for @hobby do |f| %>
    <p>
      <%= f.label :name %><br>
      <%= f.text_field :name %>
    </p> 
    <p>
      <%= f.submit %>
    </p>
  <% end %>
```

Add `update` method.

```
  class HobbiesController < ApplicationController
    ...
    def update
      @hobby = Hobby.find(params[:id])
      @hobby.update(params.require(:hobby).permit(:name))
      redirect_to @hobby
    end
    ...
  end
```

Add the `Edit` link to `app/views/hobbies/index.html.erb`, within the loop.

```
  link_to 'Edit', edit_hobby_path(hobby)
```

### Destroy a Hobby

Add `delete` method.

```
  class HobbiesController < ApplicationController
    ...
    def destroy
      @hobby = Hobby.find(params[:id])
      @hobby.destroy
      redirect_to hobbies_path
    end
    ...
  end
```

Add the `Delete` link to `app/views/hobbies/index.html.erb`, within the loop.

```
  link_to 'Delete', hobby_path(hobby), method: :delete, data: { confirm: 'Are you sure?' }
```

## Exercise

### Add More Fields

- Hobby Type [outdoor, indoor]
- Number of People
- Hours Spent
- Hours Budgeted

## Bootstrap