---
layout:       post
title:        "Understanding Law of Demeter"
subtitle:     "Understanding Law of Demeter"
date:         2017-12-29 00:00:00
author:       "Octos"
header-img:   "img/in-post/law-of-demeter/neighborhood.jpg"
catalog:      true
header-mask:  0.5
tags:
    - Design
    - Pratices
    - Object Oriented Programming
    - Tutorial
    - Beginners
---

During my studies of object oriented programming and good practices, one law/practice have gained my attention, and this was the Law of Demeter (LoD). But why this law have gained my attention so easy? Simple, when I read the principles of LoD I remembered a lot of codes, that I have coded that breaks this law.

## What is?

(Short Answer) - Basically LoD say that your object can only talk with their neighbors. On some OOP languages, for simplify just think "Use only one dot".

Law of Demeter (LoD) is a design guideline that was proposed by Ian Holland in 1987. Remember when your mom told you to not talk with strangers? It's almost the same, but with your objects.

Imagine that you have an object, this object can only talk with your methods, or with your neighbors, you should avoid call method of a object returned by another method.

At programming languages that uses dot as field identifier, you can simple use the law of "use only one dot".

For example, the fallowing code breaks the law, and throw it in the trash:

```ruby
class Book
  ..
end

class ExampleClass
  def show_book_author_name
    book.author.name
  end

  ...
end
```

The fallowing code shows the most common way to not break the law:

```ruby
class Book
  def author_name
    author.name
  end

  ...
end
```

## Benefits

It's simple, fast and brings many benefits to your code health. The benefits of fallow this law is:

* You decrease dependencies of your class (loose coupling)
* Your code is more adaptable and maintainable

*And remember less coupling, less chance of breaking your application. It's always good avoid problems.*

## Caution

Spread the rule on your project doesn't work, sometimes your design is wrong, and law of demeter will not save your project of dependencies problem, you may just be changing the place of the problem.

## Ending

That’s all for today folks, don’t forget to follow my blog and my
[twitter](https://twitter.com/opvaladares).
