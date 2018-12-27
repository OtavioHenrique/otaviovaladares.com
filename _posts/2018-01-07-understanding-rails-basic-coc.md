---
layout:       post
title:        "Understanding rails basic CoC under scaffold for begginers"
subtitle:     "Simple tutorial for begginer about rails CoC"
date:         2018-01-07 22:00:00
author:       "Otavio H. P. Valadares"
header-img:   "img/in-post/understanding-coc-under-sacaffold/scaffold.jpg"
header-mask:  0.5
catalog:      true
multilingual: true
tags:
    - Rails
    - Ruby
    - API
    - Tutorial
    - Beginners
---

Rails is a framework where almost everything is based on convention over configuraton (CoC) philosofy. Many begginers with rails framework don't understand exactly what rails do with CoC to build applications.

This is a tutorial for begginers only, if you're not new in ruby/rails development, close this tab, I will explain exactly what rails do on a basic CRUD flow based on what rails do when creating a scaffold.

The code used at this tutorial can be found [here](https://github.com/OtavioHenrique/scaffold_example)

## Creating the example

First of all, create and rails application with any name, on this tutorial I will use library (With sqlite because its dont matter here).

`
rails new library
`

(If you usually read my blog, at this point you already realized that I use library example on every tutorial, I will use other exemple on my next tutorial, I promise)


Now, you're with a new application without any controller or model (except application).

Our focus on this tutorial will be on interaction between model-view-controller while creating a simple CRUD.

At this time, rails already created all project structure and our folders are this:

```
app
├── assets
├── channels
├── controllers
│   ├── application_controller.rb
│   └── concerns
│       └── .keep
├── models
│   ├── application_record.rb
│   └── concerns
│       └── .keep
└── views
    └── layouts
        ├── application.html.erb
        ├── mailer.html.erb
        └── mailer.text.erb
```


On my opinion the most simple way of understand and see what rails do with CoC philosofy  is using a simple rails command line tool and see what files rails create and where.

`rails g scaffold book isbn:string name:string author:string description:text`

Rails will output something like this:

```ruby
      invoke  active_record
      create    db/migrate/20171225225129_create_books.rb
      create    app/models/book.rb
      invoke    test_unit
      create      test/models/book_test.rb
      create      test/fixtures/books.yml
      invoke  resource_route
       route    resources :books
      invoke  scaffold_controller
      create    app/controllers/books_controller.rb
      invoke    erb
      create      app/views/books
      create      app/views/books/index.html.erb
      create      app/views/books/edit.html.erb
      create      app/views/books/show.html.erb
      create      app/views/books/new.html.erb
      create      app/views/books/_form.html.erb
      invoke    test_unit
      create      test/controllers/books_controller_test.rb
      invoke    helper
      create      app/helpers/books_helper.rb
      invoke      test_unit
      invoke    jbuilder
      create      app/views/books/index.json.jbuilder
      create      app/views/books/show.json.jbuilder
      create      app/views/books/_book.json.jbuilder
      invoke  test_unit
      create    test/system/books_test.rb
      invoke  assets
      invoke    coffee
      create      app/assets/javascripts/books.coffee
      invoke    scss
      create      app/assets/stylesheets/books.scss
      invoke  scss
      create    app/assets/stylesheets/scaffolds.scss

```

## Understading what rails do with Scaffold

At this time, let's look carefully to every thing that rails do on last command, starting with first lines.

      invoke  active_record
      create    db/migrate/20171225225129_create_books.rb
      create    app/models/book.rb
      invoke    test_unit
      create      test/models/book_test.rb
      create      test/fixtures/books.yml

Rails invoked active record to generate a migration file with the fields that we specified on scaffold command and created a model book and a test for him.

     invoke  resource_route
     route   resources :books

On the next lines (the above lines), rails created a `resources` routes for books.

*Resource routing allows you to quickly declare all of the common routes for a given resourceful controller. Instead of declaring separate routes for your index, show, new, edit, create, update and destroy actions, a resourceful route declares them in a single line of code. See more: http://guides.rubyonrails.org/routing.html#resource-routing-the-rails-default*

If you understood what `resources` do at routes you already realized that rails will search for a controller named book when receive a request for `/books`, and that is exactly what rails created on the next steps of scaffold:

```
    invoke  scaffold_controller
    create    app/controllers/books_controller.rb
```

But if you open this file, you will see a complete controller with all methods that `resources` do.

```ruby
class BooksController < ApplicationController
  before_action :set_book, only: [:show, :edit, :update, :destroy]

  # GET /books
  # GET /books.json
  def index
    @books = Book.all
  end

  # GET /books/1
  # GET /books/1.json
  def show
  end

  # GET /books/new
  def new
    @book = Book.new
  end

  # GET /books/1/edit
  def edit
  end

  # POST /books
  # POST /books.json
  def create
    @book = Book.new(book_params)

    respond_to do |format|
      if @book.save
        format.html { redirect_to @book, notice: 'Book was successfully created.' }
        format.json { render :show, status: :created, location: @book }
      else
        format.html { render :new }
        format.json { render json: @book.errors, status: :unprocessable_entity }
      end
    end
  end

  # PATCH/PUT /books/1
  # PATCH/PUT /books/1.json
  def update
    respond_to do |format|
      if @book.update(book_params)
        format.html { redirect_to @book, notice: 'Book was successfully updated.' }
        format.json { render :show, status: :ok, location: @book }
      else
        format.html { render :edit }
        format.json { render json: @book.errors, status: :unprocessable_entity }
      end
    end
  end

  # DELETE /books/1
  # DELETE /books/1.json
  def destroy
    @book.destroy
    respond_to do |format|
      format.html { redirect_to books_url, notice: 'Book was successfully destroyed.' }
      format.json { head :no_content }
    end
  end

  private
    # Use callbacks to share common setup or constraints between actions.
    def set_book
      @book = Book.find(params[:id])
    end

    # Never trust parameters from the scary internet, only allow the white list through.
    def book_params
      params.require(:book).permit(:isbn, :name, :author, :description)
    end
end
```

And this is part of rails scaffold magic, he generate the code of the controller, and this only work because of CoC that rails implements.

At this point, we already have the model and Controller, and on the next lines of scaffold we will have the views, completing the mvc design.

```
      invoke      erb
      create      app/views/books
      create      app/views/books/index.html.erb
      create      app/views/books/edit.html.erb
      create      app/views/books/show.html.erb
      create      app/views/books/new.html.erb
      create      app/views/books/_form.html.erb
 ```



 At this point rails created all the views that correspond for each action of the books controller, following the specified convention.

 Rails documentation of action view for better understanding:

 *"There is a naming convention for views in Rails. Typically, the views share their name with the associated controller action, as you can see above. For example, the index controller action of the articles_controller.rb will use the index.html.erb view file in the app/views/articles directory. The complete HTML returned to the client is composed of a combination of this ERB file, a layout template that wraps it, and all the partials that the view may reference. Within this guide you will find more detailed documentation about each of these three components.
See more: http://guides.rubyonrails.org/action_view_overview.html"*


Now, everything is done, we have the complete MVC. If you start your applications (you'll need to run your migrations) and go to `http://localhost:3000/books` you can interact with our scaffold.

Your application should look like this:

![alt text](https://s3.amazonaws.com/garagelabio/understanding-scaffold/running-application.png "Running Application")

*(Note that I already added one book)*

## How rails pass information between controller-gview using CoC

But how everything works? How the information pass between the application? Let`s see this.

For this, we will use a legendary method called debug, first of all add gem `byebug` at your gemfile.

```
gem 'byebug', '~> 9.0', '>= 9.0.6'
```

After this, run `bundle` at your project folder, wait and let`s debug.

For this test we will use the `index` page of books.

But how to find where we can put our byebug? So, run `rake routes` at your project and you`ll see:

```
Prefix Verb   URI Pattern               Controller#Action
    books GET    /books(.:format)          books#index
          POST   /books(.:format)          books#create
 new_book GET    /books/new(.:format)      books#new
edit_book GET    /books/:id/edit(.:format) books#edit
     book GET    /books/:id(.:format)      books#show
          PATCH  /books/:id(.:format)      books#update
          PUT    /books/:id(.:format)      books#update
          DELETE /books/:id(.:format)      books#destroy
```

Now you can see that when we access our application on `/books` we are redirected to book controller at index action. So, when we need to put our byebug? Exact, on books controller at index action.

```ruby
class BooksController < ApplicationController
  before_action :set_book, only: [:show, :edit, :update, :destroy]

  # GET /books
  # GET /books.json
  def index
    byebug
    @books = Book.all
  end
```

Now access our application at `/books` and go to terminal window when our server is running and we'll see:

```ruby
 6:   def index
    7:     byebug
=>  8:     @books = Book.all
    9:   end
```

Now, on this line we're selecting all books using Active Record `Book.all` and storing at @books variable.

At this point `@books` is storing all books of my application in a `ActiveRecord::Relation` (But this don't metter on this tutorial).

```
#<ActiveRecord::Relation [#<Book id: 1, isbn: "978-1400096237", name: "The Information", author: "James Gleick", description: "A nice book", created_at: "2017-12-27 19:18:01", updated_at: "2017-12-27 19:21:32">]>
```

But it's only this, why are we storing this at @books? Where rails calls the view? Simple, fallowing the convention, our action is books controller, index action (fallowing what are in routes `books#index`) so rails will search for the view at `app/views/books/index.html.erb` and automatically starts it passing the `@books` variable that we created on controller.

So, open the `books#index` view file, and note that on some line (16 at my rails version), the generated code is accessing `@books` that we created on controller and using an each loop.

```ruby
<tbody>
    <% @books.each do |book| %>
      <tr>
        <td><%= book.isbn %></td>
        <td><%= book.name %></td>
        <td><%= book.author %></td>
        <td><%= book.description %></td>
        <td><%= link_to 'Show', book %></td>
        <td><%= link_to 'Edit', edit_book_path(book) %></td>
        <td><%= link_to 'Destroy', book, method: :delete, data: { confirm: 'Are you sure?' } %></td>
      </tr>
    <% end %>
  </tbody>
```

Now if you put another byebug before our loop:

```ruby
....

<% byebug %>
    <% @books.each do |book| %>
      <tr>

....
```

And make a new request, and wait for stop at this byebug:

```ruby
  17:   <% byebug %>
=> 18:     <% @books.each do |book| %>
   19:       <tr>

```

And calls for `@books` variable, we'll see the the same data at variable (obviously).

```
<ActiveRecord::Relation [#<Book id: 1, isbn: "978-1400096237", name: "The Information", author: "James Gleick", description: "A nice book", created_at: "2017-12-27 19:18:01", updated_at: "2017-12-27 19:21:32">]>
```

And that's is how rails pass data between the controller-view. For a better example I will create a new variable with my name at `books#index`:

```ruby
def index
    @books = Book.all
    @name = 'Octos'
  end
```

And I'll add a new `<h2>` with this variable at view:

```ruby
<h1>Books</h1>
<h2><%= @name %></h2>
```

And make a new request, and I'll see my name at index, because the rails pass the information of `@name` between the controller-view.

![alt text](https://s3.amazonaws.com/garagelabio/understanding-scaffold/app-with-name.png "My name at book#index")

*Disclaimer:* The variable of controller and view is not the same, you can see this puting two byebugs, one on controller and another at view and calling the `object_id` on each, the numbers will be differentent.

Controller:

```ruby
(byebug) @books.object_id
8
```

View:

```ruby
(byebug) @books.object_id
70132622813300
```

### Finishing

Everything is done, if you have any question, please ask.

The code of the example can be found [here](https://github.com/OtavioHenrique/scaffold_example)

That’s all for today folks, don’t forget to follow my blog and my
[twitter](https://twitter.com/opvaladares).
