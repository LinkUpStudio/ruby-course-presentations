## Ruby on Rails
### @css[no-text-transform]("Pt. #{prev + 1} - Routes + Controllers + Views")

---

#### The Purpose of the Rails Router

The Rails router **recognizes URLs** and **dispatches them to a controller's
action**, or to a Rack application. It can also **generate paths and URLs**,
avoiding the need to _hardcode strings in your views_.

https://guides.rubyonrails.org/routing.html

+++

When your Rails application receives an incoming request for:

`GET /patients/17`

it asks the **router to match it to a controller action**.
If the **first matching** route is:

```ruby
get '/patients/:id', to: 'patients#show'
```

the request is dispatched to the **patients controller's show** action
(`PatientsController#show` method) with `{ id: '17' }` in `params`.

+++

You can also **generate paths and URLs**. If the route above is modified to be:

```ruby
get '/patients/:id', to: 'patients#show', as: 'patient'
```

On the view you can use:

```
<%= link_to 'Patient Record', patient_path(patient) %>
```

then the router will generate the path `/patients/17`
(if `id` of your `patient` was `17`)

+++

##### Restful (resourceful) syntax

What does it generate?

```ruby
resources :photos
```

+++

creates **seven** different routes in your application, all mapping to
`PhotosController`:


HTTP Verb  | Path | Controller#Action | Used for
---------- | ---- | ----------------- | --------
GET        | /photos | photos#index | display a list of all photos
GET        | /photos/new | photos#new | return an HTML form for creating a new photo
POST       | /photos | photos#create | create a new photo
GET        | /photos/:id | photos#show | display a specific photo
GET        | /photos/:id/edit | photos#edit | return an HTML form for editing a photo
PATCH/PUT  | /photos/:id | photos#update | update a specific photo
DELETE     | /photos/:id | photos#destroy | delete a specific photo

+++

also creates the following Path and URL Helpers:

@ul[custom-list]
  - `photos_path` returns `"/photos"`
  - `new_photo_path` returns `"/photos/new"`
  - `edit_photo_path(:id)` returns `"/photos/:id/edit"` (for instance, `edit_photo_path(10)` returns `"/photos/10/edit"`)
  - `photo_path(:id)` returns `"/photos/:id"` (for instance, `photo_path(10)` returns `"/photos/10"`)
@ulend

+++

you can cherry-pick for which exact actions you want to generate routes:

```ruby
resources :photos, only: %i[index show]
```

+++

##### Singular resource

(there can be the one and only record for it)

```ruby
resource :geocoder
```

+++

creates **six** different routes in your application, all mapping to the
`GeocodersController`:

HTTP Verb  | Path | Controller#Action | Used for
---------- | ---- | ----------------- | --------
GET        | /geocoder/new | geocoders#new | return an HTML form for creating the geocoder
POST       | /geocoder | geocoders#create | create the new geocoder
GET        | /geocoder | geocoders#show | display the one and only geocoder resource
GET        | /geocoder/edit | geocoders#edit | return an HTML form for editing the geocoder
PATCH/PUT  | /geocoder | geocoders#update | update the one and only geocoder resource
DELETE     | /geocoder | geocoders#destroy | delete the geocoder resource

+++

Because you might want to use the **same controller** for a singular route
(/account) and a plural route (/accounts/45), **singular resources map to plural
controllers**. So that, for example, `resource :photo` and `resources :photos`
creates both singular and plural routes that map to the same controller
(`PhotosController`).

+++

A singular resourceful route generates these helpers:

@ul[custom-list]
  - `new_geocoder_path` returns `"/geocoder/new"`
  - `edit_geocoder_path` returns `"/geocoder/edit"`
  - `geocoder_path` returns `"/geocoder"`
@ulend

+++

to see all your generated routes use console command <br>
`rails routes`

which prints something like this:
```
users GET    /users(.:format)          users#index
      POST   /users(.:format)          users#create
new_user GET    /users/new(.:format)      users#new
edit_user GET    /users/:id/edit(.:format) users#edit
```

+++

Read at home:

- [2.6 Controller Namespaces and Routing](https://guides.rubyonrails.org/routing.html#controller-namespaces-and-routing)
- [2.7 Nested Resources](https://guides.rubyonrails.org/routing.html#nested-resources) especially about most useful **Shallow Nesting**
- [2.10 Adding More RESTful Actions](https://guides.rubyonrails.org/routing.html#adding-more-restful-actions)
- [3 Non-Resourceful Routes](https://guides.rubyonrails.org/routing.html#non-resourceful-routes)
- [4 Customizing Resourceful Routes](https://guides.rubyonrails.org/routing.html#customizing-resourceful-routes)

well, basically everything :D

---

#### A bit about Views

+++

**By default** for each **action** Rails will try to `render` _corresponding_
view (template).

For example, for action `BooksController#new`, Rails will try to render template
file under path `app/views/books/new.html.erb`.

+++

quick view example

```
<h1>Listing Books</h1>

<table>
  <thead>
    <tr>
      <th>Title</th>
      <th>Content</th>
      <th colspan="3"></th>
    </tr>
  </thead>

  <tbody>
    <% @books.each do |book| %>
      <tr>
        <td><%= book.title %></td>
        <td><%= book.content %></td>
        <td><%= link_to "Show", book %></td>
        <td><%= link_to "Edit", edit_book_path(book) %></td>
        <td><%= link_to "Destroy", book, method: :delete, data: { confirm: "Are you sure?" } %></td>
      </tr>
    <% end %>
  </tbody>
</table>

<br>

<%= link_to "New book", new_book_path %>
```
@[12]()
@[13]()
@[14-20]()
@[21]()
@[22]()

+++

quick form example

```
<%= form_for @article, html: {class: "nifty_form"} do |f| %>
  <%= f.text_field :title %>
  <%= f.text_area :body, size: "60x12" %>
  <%= f.submit "Create" %>
<% end %>
```

+++

Read at home:

https://guides.rubyonrails.org/action_view_overview.html

https://guides.rubyonrails.org/layouts_and_rendering.html

https://guides.rubyonrails.org/form_helpers.html
