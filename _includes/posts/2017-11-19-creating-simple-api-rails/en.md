On this post I’ll teach how to make an rest api in few minutes using rails 5 api
only, I know this is to late (ruby 5 was released in more than year ago), but
I’ll try to make a good tutorial that need only some minutes and only one post.

Let’s start!

*Note: This post don’t use TDD or any test, but everybody knows that it’s
necessary, try to make your own tests.*

*In this tutorial you need rails 5+ installed.*

### 2. Creating and Setting Up

First of all, we will start a new rails project with mysql database using the
api mode introduced on rails 5. I passed -T flag to skip test unit because I’ll
use rspec on this project, but if you prefer Minitest you can use this.

`rails new library --api -d mysql -T`

#### 2.1 Installing Rspec

Accessing the [github of rspec](https://github.com/rspec/rspec-rails).

For install just add to your GEMFILE on the development/test group:

```ruby
group :development, :test do
  gem 'rspec-rails', '~> 3.6'
end
```

And to avoid future work, add [factory
girl](https://github.com/thoughtbot/factory_girl) to your gemfile too.

```ruby
gem 'factory_girl'
```

After this, just run `bundle install` and wait the end of processs, after this,
run `rails generate rspec:install` and you'll see rspec creating his folders and
files.

*Rails output*

    create  .rspec
    create  spec
    create  spec/spec_helper.rb
    create  spec/rails_helper.rb

After this we finished the configuration of our test suit.

#### 2.2 Setting API subdomain

Open your routes.rb and I think it will look like this:

```ruby
Rails.application.routes.draw do
end
```
And replace to this:

```ruby
Rails.application.routes.draw do
  constraints subdomain: 'api' do
  end
end
```

With this we’re telling to rails that our application can be accessed on url
`api.<some_name>.com/` (With `api` subdomain).

#### 2.3 Set up /etc/hosts

To work with subdomain on localhost (development), you need to set up the
subdomain on `/etc/hosts` (On UNIX base SO’s) to receive you api subdomain +
domain.

Open you terminal and open the /etc/hosts file with sudo permission, I
personally prefer to do this with vim, but feel free to choose what you prefer.

After open your /etc/hosts simple add the fallowing lines:

    127.0.0.1 api.library.com

And that’s all. Now you can develop using curl/postman easily.

*Note: If you’re using windows, please fallow this
*[tutorial](https://www.howtogeek.com/howto/27350/beginner-geek-how-to-edit-your-hosts-file/)*.*

#### 2.4 Setting API Versioning

Open your GEMFILE to add the fallowing gems:

```ruby
# A plugin for versioning Rails based RESTful APIs.
gem 'versionist'
```
And just run `bundle install` and wait. After installing your dependencies we
can configure our project to work with versionist gem, this gem easily provide
api versioning for our project, and I'll show you.

If you’re the guy who prefer to create things by command line, I don’t hate you,
and you can make everything I’m going to do next with the fallowing line:

`rails generate versionist:new_api_version v1 V1 --path=value:v1`

*Note that if you do with this generator rails will create a lot of things for
you, not just the route*

*Rails output*

    create  app/controllers/v1
    create  app/controllers/v1/base_controller.rb
    create  spec/controllers/v1
    create  spec/controllers/v1/base_controller_spec.rb
    create  spec/requests/v1
    create  spec/requests/v1/base_controller_spec.rb
    create  app/presenters/v1
    create  app/presenters/v1/base_presenter.rb
    create  spec/presenters/v1
    create  spec/presenters/v1/base_presenter_spec.rb
    create  app/helpers/v1
    create  spec/helpers/v1
    create  public/docs/v1
    create  public/docs/v1/index.html
    create  public/docs/v1/style.css

*But if you do that your routes will look like this:*

```ruby
constraints subdomain: 'api' do
end
api_version(:module => "V1", :path => {:value => "v1"}) do
end
```

And you will need to put:

```ruby
api_version(:module => "V1", :path => {:value => "v1"}) do
end
```

Inside your namespace. But if you want to do with your fingers just write this
`api_version(...)` with your hands!

### 3.0 Creating book model

Our first model will be book, the core of the library. Our book table will have
some attributes, and will look like this:

idtitleauthorweightpublisherlanguagepagespublicationDate1Atlas ShruggedAyn
Rand3.4 poundsDuttonEnglish1168April 21, 20052Murder on the Orient ExpressAgatha
Christie12.6 ouncesWilliam MorrowEnglish272October 24, 2017

For create this automatically just type:

`
rails g model Book title:string author:string weight:string publisher:string language:string pages:integer publication_date:datetime
`

*Rails output*

    invoke  active_record
    create    db/migrate/20171018035914_create_books.rb
    create    app/models/book.rb
    invoke    rspec
    create      spec/models/book_spec.rb

So now, rails created the model, the migration and the spec for us.

Run `rake db:create` for create the database, and after this, run `rake
db:migrate` to create book table. Now you can see your file at
`app/models/book.rb` path.

#### 3.1 Index All Books

Go to your `routes.rb` and add:

```ruby
resources :books
```

After this, create the books controller inside version 1

Books controller path: `app/controllers/v1/books_controller.rb`

After this, enter in you file and type:

```ruby
# frozen_string_literal: true

class V1::BooksController < ApplicationController
  def index
    books = Book.all
    render json: { books: books }, status: :ok
  end
end
```

and we’re done, but for test we need to create some books on database. Type
`rails c` on your terminal and let's do that.

`
Book.create(title: "Atlas Shrugged", author: "Ayn Rand", weight: "3.4 pounds", publisher: "Dutton", language: "English", pages: 1168, publication_date: "April 21, 2005")
`

Feels free to alter this data and create different books for you.

Now just start your rails server and go to your postman and `GET
http://api.library.com:3000/v1/books` and you will see the API returning your
book data, like this(using my data):

*API output*

    {
        "books": [
            {
                "id": 1,
                "title": "Atlas Shrugged",
                "author": "Ayn Rand",
                "weight": "3.4 pounds",
                "publisher": "Dutton",
                "language": "English",
                "pages": 1168,
                "publication_date": "2005-04-21T00:00:00.000Z",
                "created_at": "2017-10-20T02:39:53.000Z",
                "updated_at": "2017-10-20T02:39:53.000Z"
            },
            {
                "id": 2,
                "title": "Murder on the Orient Express",
                "author": "Agatha Christie",
                "weight": "12.6 ounces",
                "publisher": "12.6 ounces",
                "language": "English",
                "pages": 272,
                "publication_date": "2017-10-24T00:00:00.000Z",
                "created_at": "2017-10-20T02:40:15.000Z",
                "updated_at": "2017-10-20T02:40:15.000Z"
            }
        ]
    }

And we’re done. We finished our “Hello world” in rest APIs.

### 4.0 Active Model Serializer

Active model serializer it’s a fabulous gem, thats brings convention over
configuration to our API’s JSON, he works with serializers and adapters and I’ll
show you how to use.

First of all, open your gemfile and add:

```ruby
gem 'active_model_serializers', '~> 0.10.0'
```

And run `bundle`

Afther this, you can create any serializer with the fallowing command (On the
example I’m creating to book model):

`rails g serializer v1/book`

*Rails output*

    create  app/serializers/v1/book_serializer.rb

Now, go to your v1/books_controller.rb and on your index action, remove `json: {
books: books }` and put just `json: books` for we start using the AMS.

The final code of the index action of books_controller.rb will be like:

```ruby
def index
  books = Book.all
  render json: books, status: :ok
end
```

Now if we restart our server and make the same `GET
http://api.library.com:3000/v1/books` will we see this output:

*API output*

    [
         {
             "id": 1
         },
         {
             "id": 2
         }
     ]

Don’t panic, this is normal! We’ll see only Ids of the books because we don’t
set up our books serializer to output others attributes, to fix this, open your
`app/serializers/v1/book_serializer.rb` and you will see only this:

```ruby
class V1::BookSerializer < ActiveModel::Serializer
  attributes :id
end
```

The only attribute that is outputing is `id` and we need to add more things, add
the others things of the model that you want to show (I will add everything
except `created_at` and `updated_at` because I don't think necessary), and my
code will look like:

```ruby
class V1::BookSerializer < ActiveModel::Serializer
  attributes :id, :title, :author, :weight, :publisher, :language, :pages, :publication_date
end
```

Now if we call our API again, we will get:

*API output*

    [
        {
            "id": 1,
            "title": "Atlas Shrugged",
            "author": "Ayn Rand",
            "weight": "3.4 pounds",
            "publisher": "Dutton",
            "language": "English",
            "pages": 1168,
            "publication_date": "2005-04-21T00:00:00.000Z"
        },
        {
            "id": 2,
            "title": "Atlas Shrugged",
            "author": "Ayn Rand",
            "weight": "3.4 pounds",
            "publisher": "Dutton",
            "language": "English",
            "pages": 1168,
            "publication_date": "2005-04-21T00:00:00.000Z"
        }
     ]

Looks pretty much sexy, right?

#### 4.1 Custom Methods On Serializer

One thing is troubling me on this response, it’s publication_date format, look
on the last json, `2005-04-21T00:00:00.000Z` is not good for me, let's fix this,
on the `BookSerializer` add this method:

```ruby
def publication_date
  object.publication_date.strftime "%Y-%m-%d"
end
```

With this, we’re taking the `object` that is book record in this context and
formating date, remember that you can add any custom method using this, just
remeber to add in the attributes on the top of your serializer and make your
custom method.

Now our json is looking like this:

*API output*

    [
        {
            "id": 1,
            "title": "Atlas Shrugged",
            "author": "Ayn Rand",
            "weight": "3.4 pounds",
            "publisher": "Dutton",
            "language": "English",
            "pages": 1168,
            "publication_date": "2005-04-21"
        },

       ....
    ]

#### 4.2 Json API Adapter

One of the advantages of use active model serializer gem is that this gem
alterady have an adapter to use [jsonapi.org](http://jsonapi.org/) convention in
your API, for whos don’t know, this shared conventions pattern the json
resposes. I really recommend that you read more about
[here](http://jsonapi.org/).

Create a file named `active_record_serializer.rb` at `config/initializers/` and
put this inside:

```ruby
    ActiveModelSerializers.config.adapter = :json_api
```

Restart your application, make the same call for your API and see the
difference.

*API output*

    {
        "data": [
            {
                "id": "1",
                "type": "books",
                "attributes": {
                    "title": "Atlas Shrugged",
                    "author": "Ayn Rand",
                    "weight": "3.4 pounds",
                    "publisher": "Dutton",
                    "language": "English",
                    "pages": 1168,
                    "publication-date": "2005-04-21"
                }
            },
            {
                "id": "2",
                "type": "books",
                "attributes": {
                    "title": "Atlas Shrugged",
                    "author": "Ayn Rand",
                    "weight": "3.4 pounds",
                    "publisher": "Dutton",
                    "language": "English",
                    "pages": 1168,
                    "publication-date": "2005-04-21"
                }
            }
        ]    
    }

Your api response are now fallowing jsonapi specification without pains.

### 5. 0 Creating the others actions (Create, Show, PUT/Patch, Delete)

Now that I explained all the things of this project architecture we just need to
finish our CRUD actions and and we’re done. We already have our routes created
because we write `resources :books` at our routes.rb, so if you run the `rake
routes`on the project we will have:

*Rails output*

    Prefix Verb   URI Pattern             Controller#Action
    v1_books GET    /v1/books(.:format)     v1/books#index {:subdomain=>"api"}
             POST   /v1/books(.:format)     v1/books#create {:subdomain=>"api"}
     v1_book GET    /v1/books/:id(.:format) v1/books#show {:subdomain=>"api"}
             PATCH  /v1/books/:id(.:format) v1/books#update {:subdomain=>"api"}
             PUT    /v1/books/:id(.:format) v1/books#update {:subdomain=>"api"}
             DELETE /v1/books/:id(.:format) v1/books#destroy {:subdomain=>"api"}

#### 5.1 Action Create

Let’s start with creating action of our books. Since we already have the create
route created we just need to code create action on, `v1/books_controller.rb`,
open this file, and write:

```ruby
    def create
      book = Book.new(book_params)

      if Book.new(book_params)
        render json: book, status: :created
      else
        render json: { errors: book.errors }, status:    :unprocessable_entity
      end

      private

      def book_params
        params.require(:book).permit(:title, :author, :weight, :publisher, :language, :pages, :publication_date)
      end
```

Go yo your postman to test our new action, and `POST
http://api.library.com:3000/v1/books` and make a request with this body:

*API output*

    {
        "book": {
    	"title": "The Information: A History, a Theory, a Flood",
    	"author": "James Gleick",
    	"weight": "1.2 pounds",
    	"publisher": "Vintage",
    	"language": "English",
    	"pages": 544,
    	"publication_date": "March 6, 2012"
        }
    }

And we will got 201 status response and a json with our new book information,
now if you go to your `rails c` and type `Book.last` you will get our book
created:

*Rails output*

    => #<Book id: 5, title: "The Information: A History, a Theory, a Flood", author: "James Gleick", weight: "1.2 pounds", publisher: "Vintage", language: "English", pages: 544, publication_date: "2012-03-06 00:00:00", created_at: "2017-10-21 19:12:33", updated_at: "2017-10-21 19:12:33">

*Note: I pretty recommend that if you are a begginer, make every time this
tests, it’s cool to see your application running.*

#### 5.2 Action Show

The same thing of action create, but you need to write this at controller:

```ruby
def show
  book = Book.find(params[:id])
  render json: book, status: :ok
end
```

And go to your postman and make this request: `GET
http://api.library.com:3000/v1/books/1` and you will see your `id: 1` book on
response. (You can pass any valid id and return the corresponding book).

*Note: on the final of request you’re passing the id of your book, on this
example I used 1.*

*API output*

    {
        "data": {
            "id": "5",
            "type": "books",
            "attributes": {
                "title": "The Information: A History, a Theory, a Flood",
                "author": "James Gleick",
                "weight": "1.2 pounds",
                "publisher": "Vintage",
                "language": "English",
                "pages": 544,
                "publication-date": "2012-03-06"
            }
        }
    }

*Note: You can find every book of your database, just pass a valid id.*

#### 5.3 Action Update

Update it’s like the action create with some changes:

```ruby
def update
    book = Book.find(params[:id])

    if book.update(book_params)
      render json: book, status: :ok
    else
      render json: { errors: book.errors }, status: :unprocessable_entity
    end
  end
```

Now just `PUT http://api.library.com:300/v1/books/5` with this json:

    {
    	"publisher": "Vintage Books"
    }

This json with this request will change the punblisher of the book with `id: 5`
in my database to "Vintage Books" (Actually is only "Vintage"), make this
request and see the response.

*NOTE: On this example I used the *`id: 5`* book because on my database was this
id, on your database depending of what you done, this id can change. You can do
this example with any book of your database, just change for something that you
want.*

*API output*

    {
        "data": {
            "id": "5",
            "type": "books",
            "attributes": {
                "title": "The Information: A History, a Theory, a Flood.",
                "author": "James Gleick",
                "weight": "1.2 pounds",
                "publisher": "Vintage Books",
                "language": "English",
                "pages": 544,
                "publication-date": "2012-03-06"
            }
        }
    }

#### 5.4 Action Destroy

Destroy it’s the easiest action of the CRUD, you just need to write:

```ruby
def destroy
  book = Book.find(params[:id])
  book.destroy
  head 204
end
```

And that’s all, to test, make a request `DELETE
http://api.library.com:3000/v1/books/2` and you will delete the second book of
your database. The API will not return anything but the request status will be
204. If you go to your rails console and try to find the book with `id: 2`,
using `Book.find 2` you will get this error:

    ActiveRecord::RecordNotFound: Couldn't find Book with 'id'=2

Telling that this book dont exist anymore.

**Note: We can’t revert this action. “Remember, with great power comes great
responsibility”**

### Finishing

Everything is done, if you have any question, please ask.

That’s all for today folks, don’t forget to follow my blog and my
[twitter](https://twitter.com/ValadaresOtavio).
