## Continue a Rails App

Go through the App and features built so far.

Ask questions.

-----


Add `paperclip` to `Gemfile`.

```
gem 'paperclip'
```

Install the gem.

```
> bundle install
```

Add an `image` column to the `hobbies` table.

```
> rails generate paperclip hobby image
```

Run the migration.

```
> rake db:migrate
```

----

Configure the App to allow for upload.

### Model

`app/models/hobby.rb`

```
  class Hobby < ActiveRecord::Base
    has_attached_file :image, styles: { medium: "300x300#", small: "100x100#" }
    validates_attachment_content_type :image, :content_type => /\Aimage\/.*\Z/
  end
```

### View

`app/views/hobbies/new.html.erb`

```
  <h1>New Hobby</h1>
  <%= form_for :hobby, url: hobbies_path do |f| %>
    <div>
      <%= f.label :name %><br>
      <%= f.text_field :name %>
    </div>
    <div>
      <%= f.label :image %><br>
      <%= f.file_field :image %>
    </div>
    <div>
      <%= f.submit %>
    </div>
  <% end %>
  <br>
  <%= link_to "Back to Hobbies", hobbies_path %>
```

----

Try uploading an image! Now let's display the image.

`app/views/hobbies/show.html.rb`

```
  <p>
    <strong>Name:</strong>
    <%= @hobby.name %>
    <br>
    <%= image_tag @hobby.image.url %>
    <%= image_tag @hobby.image.url(:medium) %>
    <%= image_tag @hobby.image.url(:small) %>
  </p>
  <%= link_to 'Back to Hobbies', hobbies_path %>
```

`app/views/hobbies/index.html.rb`

```
  <h1>My Hobbies</h1>
  <% @hobbies.each do |hobby| %>
    <p>
      <%= hobby.id %>: <%= hobby.name %>
      <%= image_tag hobby.image.url(:small) %>
      <%= link_to "delete", hobby, method: :delete, data: { confirm: "Are you sure?" } %>
    </p>
  <% end %>
  <hr>
  <%= link_to "New Hobby", new_hobby_path %>
```

----

## Beautify Buttons

`app/views/hobbies/index.html.rb`

```
  <%= link_to "New Hobby", new_hobby_path, class: 'btn btn-primary btn-lg' %>
```

```
  <%= link_to "delete", hobby, method: :delete, data: { confirm: "Are you sure?" }, class: 'btn btn-danger btn-xs' %>
```
	
Do it for `link` in:

- `app/views/hobbies/new.html.rb`
- `app/views/hobbies/show.html.rb`

```
  <%= f.submit class: 'btn btn-success' %>
  <%= link_to "Back to Hobbies", hobbies_path, class: 'btn btn-warning btn-sm' %>
```

---

## Style Hobbies Page
	
`app/views/hobbies/index.html.rb`

```
  <div>
    <%= link_to "New Hobby", new_hobby_path, class: 'btn btn-primary btn-lg pull-right' %>
    <h1>My Hobbies</h1>
  </div>
  <hr>
  <div class='row'>
    <% @hobbies.each do |hobby| %>
      <div class="col-sm-3">
        <div class="thumbnail">
          <%= image_tag hobby.image.url(:medium) %>
          <div class="caption">
            <h3><span class="glyphicon glyphicon-heart"></span> <%= hobby.name %></h3>
            <%= link_to "delete", hobby, method: :delete, data: { confirm: "Are you sure?" }, class: 'btn btn-danger btn-xs' %>
          </div>
        </div>
      </div>
    <% end %>  
  </div>  
```

> http://getbootstrap.com/components/#glyphicons

## Style Form

`app/views/hobbies/index.html.rb` - Add `.form-group` and `.form-control`

```
  <h1>New Hobby</h1>
  <hr>
  <%= form_for :hobby, url: hobbies_path do |f| %>
    <div class="form-group">
      <%= f.label :name %><br>
      <%= f.text_field :name, class: 'form-control' %>
    </div>
    <div class="form-group">
      <%= f.label :image %><br>
      <%= f.file_field :image, class: 'form-control' %>
    </div>
    <div class="form-group">
      <%= f.submit class: 'btn btn-success' %>
    </div>
  <% end %>
  <br>
  <%= link_to "Back to Hobbies", hobbies_path, class: 'btn btn-warning btn-sm' %>
```

----

Sign the App on `app/views/layouts/application.html.erb`

```
  <footer class='text-center'>
    <hr>
    Built with <span class='glyphicon glyphicon-heart'></span> by Winston
  </footer>
```

----

## Rating

Add `ratings` to `hobbies` table.

```
> rails generate migration add_ratings_to_hobbies ratings:integer
```

Run the migration.

```
> rake db:migrate
```

Add `ratings` field to the form.

```
  # new.html.erb
  <div class="form-group">
    <%= f.label :ratings %><br>
    <%= f.select :ratings, 1..5, {}, class: 'form-control' %>
  </div>
```

Show `ratings`.

```
  # show.html.erb
  <%= @hobby.ratings %>
```

```
  # index.html.erb
  <p>Ratings: <%= hobby.ratings %></p>
```

Get `Average Rating`.

```
  <hr>
  <h3>Total   Ratings: <%= @hobbies.sum(:ratings) %></h3>
  <h3>Average Ratings: <%= @hobbies.sum(:ratings)/@hobbies.count.to_f %></h3>
```

----

- `<h1><%= pluralize(@hobbies.count, 'Hobby') %></h1>`
- `<p>Created <%= distance_of_time_in_words_to_now(hobby.created_at) %> ago</p>`
- Change background color?
- Change fonts?
