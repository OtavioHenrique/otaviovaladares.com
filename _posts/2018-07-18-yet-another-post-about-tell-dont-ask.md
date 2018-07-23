---
layout:       post
title:        "Yet, another post about tell don't ask"
subtitle:     "Stop treating objects like C structures"
date:         2018-07-22 23:05:00
author:       "Octos"
header-img:   "img/in-post/ask-dont-tell/adt.jpg"
header-mask:  0.5
catalog:      true
multilingual: true
tags:
    - Object Oriented Programming
    - Design
    - Pratices
    - Tutorial
    - Beginners
    - Ruby
---

Tell, don't ask is a useful key concept when talking about object-oriented programming, it consists in send messages to object (whenever possible) telling object what you want and that we shouldn't ask questions to it about its state and make decisions for him, it bypass encapsulation. 

*In more abstract mindset, every object needs to trust that other object can make decisions alone, without his help.*

If you're treating objects like structs you're wrong, the key difference between they is that structs stores only data and was made to represent a record, and this is one of the key difference betwween procedural and object oriented code, objects have behavior.

## Structures vs Objects

Defining a simple People struct in C lang and asking for its name if not empty:

```c
struct People {
    int age;
    int weight;
    char name[50];
};
```

```c
struct People ronald = { name: "", age: 42, weight: 140};

if (strcmp(ronald.name, "") != 0) {
    printf("Person Name: %s\n", ronald.name);
}
```

This C code in Ruby will look something like this in Ruby:

```ruby
class People
  attr_reader :name

  def initialize(age:, weight:, name:)
    @age = age
    @weight = weight
    @name = name
  end
end

people = People.new(age: 45, weight: 140, name: "Ronald")

puts "People name: #{people.name}" unless people.name.empty?

```

The error is trating only like record, objects have behavior too.

```ruby
class People
  def initialize(age:, weight:, name:)
    @age = age
    @weight = weight
    @name = name
  end

  def name
    "People name: #{@name}" unless @name.empty?
  end
end

people = People.new(age: 42, weight: 140, name: "Ronald")

puts people.name
```

Ok.. This is just a mediocre example, we only extracted a simple logic, but it was good to see the difference between structs (only data) and objects (behavior/state).

## Another example

Ok, let's go to another example more real, let's imagine a simple system that have Books and Stock and we want to store books at stock and print every book of stock:

```ruby
class Book
  attr_reader :name, :author, :year

  def initialize(name, author, year)
    @name = name
    @author = author
    @year = year
  end
end

class Stock
  attr_reader :capacity, :maximum_capacity
  attr_accessor :books

  def initialize(books: [], capacity: 0, maximum_capacity: 10)
    @books = books
    @capacity = capacity
    @maximum_capacity = maximum_capacity
  end
end

def add_books(book, stock)
  if stock.capacity < stock.maximum_capacity
    stock.books << book
  end
end

def print_books(stock)
  stock.books.each do |book|
    puts "Name: #{book.name}, Author: #{book.author}"
  end
end

book1 = Book.new("Practical Object-Oriented Design in Ruby","Sandi Metz","2012")
book2 = Book.new("Ruby Under a Microscope","Pat Shaughnessy","2013")

stock = Stock.new

add_books(book1, stock)
add_books(book2, stock)

print_books(stock)

```

This code works good but what's the problem? This code is so procedural, let's refactor to become more OOP.

Starting from `add_books`, it's asking a lot of informations about object(stock) state for make decisions, this is wrong and the logic of add books to stock and check about stock capacity needs to belongs to stock object, with Tell don't ask thinking this logic would be written like this:

```ruby
class Stock
  attr_reader :quantity, :maximum_quantity
  attr_accessor :books

  def initialize(books: [], quantity: 0, maximum_quantity: 10)
    @books = books
    @quantity = quantity
    @maximum_quantity = maximum_quantity
  end

  def <<(book)
    books << book unless crowded
  end

  def crowded
    quantity == maximum_quantity
  end
end
```

Now if we need to one book to stock we just need to write:

```ruby
stock << book1
```

We don't need to ask questions about state and then add, all this logic is encapsulated at object, much better right?

`print_books` needs to be refactored too, let's make this logic more OOP, first of all we can create an method at `Stock` to print all books and their information.

```ruby
class Stock
  ...

  def print_books
    books.each do |book|
      puts "Name: #{book.name}, Author: #{book.author}"
    end
  end
end
```

Ok this is good, but not good yet, we can extract this logic to book class, because this output string can be part of `Book` for me, so we don't need to ask each book about its state.

```ruby
class Book
  ...

  def full_description
    "Name: #{name}, Author: #{author}, Year: #{year}"
  end
end
```

Now we refactor again our `print_books` for just call `full_description` on each book.

```ruby
class Stock
  ...

  def print_books
    books.each do |book|
      puts book.full_description
    end
  end
end
```

Much better our code now, right? If you got lost you can check full code [here](https://gist.github.com/OtavioHenrique/ba43a1bf0b78da8c5602ce481179e439), or ask me on twitter/email.

### Comparing two codes

On the first version of the code, objects was asking for state like this image:

![Objects asking for other object state](https://s3.amazonaws.com/garagelabio/tell-dont-ask/procedural.png)

Now our objects are telling what want:

![Objects telling what want](https://s3.amazonaws.com/garagelabio/tell-dont-ask/ADT.png)

Luckily on internet we've hundreds of examples in many languages, you should search for it after read this post.

[On this article from thoughtbot you can see a lot of interesting and more real world examples](https://robots.thoughtbot.com/tell-dont-ask).

Sandi Metz in her famous book [Practical Object-Oriented Design in Ruby: An Agile Primer ](https://www.amazon.com/Practical-Object-Oriented-Design-Ruby-Addison-Wesley/dp/0321721330) wrote: 

*"The distinction between a message that asks for what the sender wants and a message
that tells the receiver how to behave may seem subtle but the consequences are
significant. Understanding this difference is a key part of creating reusable classes with
well-defined public interfaces"*


## Final thoughts

This isn't easy get started thinking like this and correcting your errors, but you need to get used to not ask your object about his state and tell him what you want every that is possible, at the beginning will be hard to think like this (it's have been difficult for me), but with time you'll mastery this skill.

[I have an old post about law of demeter that can interest you](https://otaviovaladares.com/2017/12/29/law-of-demeter/)

Thanks all for read this post, don't forget to follow me on [twitter](https://twitter.com/ValadaresOtavio), any questions you can send me an email, have good days.

## Useful links

[A lot of examples from thoughtbot](https://robots.thoughtbot.com/tell-dont-ask)

[Good explanation](http://rubyblog.pro/2016/09/tell-dont-ask-principle)

[Nice article from pragprog that talk about Law of Demeter too](https://pragprog.com/articles/tell-dont-ask)

[Good blog post from Pat Shaughnessy](http://patshaughnessy.net/2014/2/10/use-an-ask-dont-tell-policy-with-ruby)

[Dave Thomas blog post about it too](https://pragdave.me/blog/2014/02/11/telling-asking-and-the-power-of-jargon.html)