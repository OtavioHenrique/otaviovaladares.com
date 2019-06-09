---
layout:       post
title:        "Single responsibility principle"
subtitle:     "Object oriented design with mastery"
date:         2019-06-09 02:04:00
author:       "Otavio H. P. Valadares"
header-img:   "img/in-post/single-responsibility/lego_blocks.jpg"
header-mask:  0.5
multilingual: true
tags:
  - Object Oriented Programming
  - Design
  - Pratices
  - Tutorial
  - Beginners
  - Ruby
  - SOLID
---

Single responsibility principle is also known as SRP is a computer programming principle which consists of classes, functions or modules can’t have more than one responsibility.

The term itself was introduced by Robert C. Martin as a part of what he calls “Principles of Object Oriented Design”.

*To be more pragmatic in the text I’ll refer only to classes, but you can consider the same for modules. At the end of the text, I’ll discuss functions.*

But what we define as responsibility? Responsibility can be described as each action that your classes are assigned to do, these actions usually have business logic and can be used many times, and the more important, exposed to changes. It leads us to the root of SRP, one class should have only one reason to change. If a class has more than one responsibility, then these responsibilities become coupled by with themselves, and changes to one can impact the other. This kind of problems results in poor designs that violate the basics of object oriented design.

Why it violates the basics of OOD? If you were asked to describe the purpose of object oriented programming in a few words, what would you answer? I would respond “easy to change”, but how to reach it?

Today we have a lot of techniques, patterns, and references to reach good design in your applications, but the most basic to get started is to follow SRP.

Classes with only one responsibility provide a portion of well-defined behavior that can be reused in any part of your application that needs through its public interface, your application will have a lot of small pieces of code called classes, exchanging messages between them, like cells in an organism providing life to your object-oriented application.

![Classes exchanging messages](https://garagelabio.s3.amazonaws.com/SRP/SRP.png)

If your classes have only one responsibility and you need to change some business logic at this responsibility, you’ll only need to change it in one place, it will be easy to change.

### What should belong to my class

Your class always need to have only one responsibility, but how to know if it is doing only one thing?

You can analyze your class name if it contains “and” or “or”, like “EmailValidatorAndSender” probably it has more than one responsibility.

```
class EmailValidatorAndSender; end
```

*You can try to describe your class in a few words too, and if you use “and” or “or” your class have two responsibilities too.*

Another thing that you can do is to analyze your class public interface, and see if it makes sense when compared with the class name and other methods.

```ruby
class EmailSender
  def valid?(email)
     # …
  end

  def send!
    #..
  end
```

Analyzing this class public interface, the method “#valid?” makes sense? It should belong to this class if we analyze its name and other methods? Of course, no!

These two examples were very easy, the real problem stands when your multi-responsibilities is hidden inside your business logic, for example:

```ruby
class ProductBuyer
  attr_reader :user, :product, :address, :payment_data

  def initialize(user, product, address, payment_data)
    @user = user
    @product = product
    @address = address
    @payment_data = payment_data
  end

  def buy!
    # ... any logic ...

    send_email if valid?

    # ... any logic ...
  end

  private

  def send_email
    EmailProvider.send(
     email: user.email,
     template: File.open("any/path/to/template.html")
    end
  end

  def valid?
    user.email =~ /\A[\w+\-.]+@[a-z\d\-]+(\.[a-z\d\-]+)*\.[a-z]+\z/i
  end
```

This class have a lot of logic, it is a class to buy products but it contains email validations and sends emails, this is dangerous! Why it is dangerous? When your multi-responsibilities are coupled with your class logic is harder to realize it, and the tendency is to you duplicate code when you need to validate email in other parts of your application, you probably will duplicate code:

```ruby
class UserRegistration
  # ...

  private

  def validate_email
    user.email =~ /\A[\w+\-.]+@[a-z\d\-]+(\.[a-z\d\-]+)*\.[a-z]+\z/i
  end
end
```

If someday you need to change the logic of email validation, you'll need to change it in two distinct parts of your code.

If it has more than one responsibility, and consequently more than one reason to change, it will lead your application to poor design and consequently harder to change.

### Design methods with a single responsibility

A well-designed class has only one responsibility composed of many methods with single responsibility! Don't create the famous sausage method with a lot of lines, behaviors and decisions. You can extract logic to private methods, it will reduce our code complexity and boost its readability.

### Side Effects of violations

Constant violation of the SRP can cause headaches and leads your application to poor design, the first effect is the developers' productivity falling down, your application will have a poor design and will not be easy to change, developers of the system probably will need more time to make changes.

If your application classes have more than one responsibility probably you can't import it and just use the behavior that you want, you’ll get a lot of undesirable code together. It's dangerous when you want to use the behavior of one class that does more than one thing, but this behavior isn't exposed through the public interface of its class because it is coupled, in this case probably your violation of SRP will end with a duplicated code.

### Silver Bullet

It's always good to remember that it isn't a silver bullet, your application design will not be good only using SRP. It is the easiest way to get started with good design practices, but only use it will not be the answer to your problems.

Like every pattern or practice, the heavy use of it without rational thinking can lead you to a disaster, don't start applying SRP everywhere, with time and experience you'll start to write code with single responsibility naturally.

## Final thought

If you have any question that I can help you, please ask! Send an email (otaviopvaladares@gmail.com), pm me on my [Twitter](https://twitter.com/opvaladares) or comment on this post!
