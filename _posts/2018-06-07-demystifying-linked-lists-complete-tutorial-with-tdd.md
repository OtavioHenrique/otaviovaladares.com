---
layout:       post
title:        "Demystifying Linked Lists / Complete tutorial/implementation with TDD"
subtitle:     "with a lot of images to easy understand"
date:         2018-06-07 22:46:00
author:       "Otavio H. P. Valadares"
header-img:   "img/in-post/linked_list/list_banner.jpg"
header-mask:  0.5
catalog:      true
multilingual: true
tags:
    - Algorithms
    - Tutorial
    - Beginners
    - Data Structures
    - Ruby
---

## This text is not recommended for

- People who have solid knowledge of *Data Structures* and *Linked Lists*
- People who don't know ruby and object oriented programming basics

## What I'll explain on this post

- What is *Linked Lists*
- What is *Dubly Linked Lists*
- How to make our Linked List Gem
- Benchmarking

## Code

All the code written with this post you can find [here](https://github.com/OtavioHenrique/linked_list)

# 1.0 Introduction

On this post I'll start a serie of posts talking about data structures implementations using Ruby. Some years ago I studied Data Structures but coding each one in `C`, I think `C` is a good language to learn because of pointers and struct, but implement in Object Oriented Language like Ruby is good for learn new concepts, plus I'll use TDD to train and learn more about `MiniTest` (Because on my day/job I always use `Rspec`).

## 1.1 What is Linked List?

Linked List is the simplest data structure in my opinion,  it consist in set of information/data linked by pointer/link to the next data, the object/struct that stores the data if often called Node/Item.

![Linked List](https://s3.amazonaws.com/garagelabio/linked_list_ruby/linked_list.png)

On this image we can see a list of nodes storing values linked by `next_node` arrow.

But on this tutorial we'll study doubly linked list that we'll see below.

## 1.2 Doubly Linked List

Doubly Linked List is the same as Liked List, the unique major difference is that doubly have one pointer to the next node and one to the previus.

![DoublY Linked List](https://s3.amazonaws.com/garagelabio/linked_list_ruby/double_linked_list.png)

Now we have all the information that we need to start coding.

## 1.3 Architeture of Appplication

Doubly linked list is a simple data structures project, we'll create a gem that implements it. We'll have two core classes, linked_list and Node.

# 2.0 Developing

## 2.1 Creating a Gem

On this tutorial we'll build our linked list in a gem like application, that's why with gem we can easily build and use the list just including gem in our IRB/Pry session.

So, first we need to create our application, for this just use the command:

`bundle gem linked_list`

After this, bundler will ask if we want to generate test with our gem, and if yes what gem we prefer, Rspec or Minitest, as I talked above on this tutorial we'll use Minitest so just write ´minitest´ and press enter. Next all that ruby ask, you can say yes and everything will be ok.

After all steps you'll see ruby creating our gem first files.

      create  linked_list/Gemfile
      create  linked_list/lib/linked_list.rb
      create  linked_list/lib/linked_list/version.rb
      create  linked_list/linked_list.gemspec
      create  linked_list/Rakefile
      create  linked_list/README.md
      create  linked_list/bin/console
      create  linked_list/bin/setup
      create  linked_list/.gitignore
      create  linked_list/.travis.yml
      create  linked_list/test/test_helper.rb
      create  linked_list/test/linked_list_test.rb
      create  linked_list/LICENSE.txt
      create  linked_list/CODE_OF_CONDUCT.md

*On this tutorial I'll not exaplain the basic of a gem architeture, if you're courius about this project structure go and search for yourself but it's not really necessary if you know object oriented programming basics*

After this, you have a linked_list directory created, now if you try to run any command to your gem, like `rake test`, it won't run. Why? Because you will get the fallowing error:

```ruby
Please fix this gemspec. (Gem::InvalidSpecificationException)
The validation error was '"FIXME" or "TODO" is not a description'
```

If you already created a gem before you know what it is, is only the rubygems telling you that our description is not completed at file `linked_list/linked_list.gemspec`. Now let's fix quickly this.. Open this file and replace the summary and description at line 12 and 13 for anything you want.

```ruby
  spec.summary       = %q{Linked List}
  spec.description   = %q{Just another one linked list gem that I've coded for study}
```

But now if you run your tests with `rake test`, it will not pass yet, because rubygems generates a fail test on our `linked_list/test/linked_list_test.rb`, if you run test you'll get the fallowing error:

```ruby
LinkedListTest#test_it_does_something_useful [/home/otavio/Documents/blog/linked_list/test/linked_list_test.rb:9]:
Expected false to be truthy.
```

So, open this file and remove the fallowing test:

```ruby
  def test_it_does_something_useful
    assert false
  end
```

After this, everything is ok, just run `rake test` and everything is ok for we start coding.

## 2.2 Creating Node Class

Now that we have our gem created, let's create our first class, this class we'll be our node class.

First of all let's start with test, creating our test and writing the basic structure of a `minitest` test.

`test/node_test.rb`

```ruby
# frozen_string_literal: true

require "minitest/autorun"

class NodeTest < Minitest::Test
end
```

Now, where to start? I like to start creating a test to assert that we can create our `Node` class.

```ruby
  def test_creating_node
    node = Node.new()

    assert_instance_of Node, node
  end
```

Now if you run this test `rake test` you'll get the fallowing error:

```ruby
  1) Error:
NodeTest#test_creating_node:
NameError: uninitialized constant NodeTest::Node
```

This error is telling that Ruby didn't find `Node` class, to fix this let's create our class and run test again.

`lib/node.rb`

```ruby
# frozen_string_literal: true

class Node
end
```

*And don't forget to add `require "node"` at linked_list.rb to don't get errors when trying to using our gem*

Now yoour test should pass right? No, if you try to run your test you'll get the same error:

```ruby
  1) Error:
NodeTest#test_creating_node:
NameError: uninitialized constant NodeTest::Node
```

But you know why? Because we missed to require our node class in our `NodeTest`, some people aren't used to require class in Ruby because Rails does it automatically, but pure Ruby no. So go to our `NodeTest` class and require our `Node` class:

`lib/node.rb`

```ruby
require "node"
```

Run again your tests, finally we'll get our test passing.

Now we need to create tests to assert that our class have attributes and can store and returning each one. (If you are familiar with languages like java, this test is to assert our getters/setters)

Let's create a test to assert that our node are storing and returning its attribute value.

```ruby
  def test_node_value
    value = 10
    node = Node.new(value: value)

    assert_equal node.value, value
  end
```

Run tests again, you'll get the fallowing error telling that we sent one value to the `node` constructor and it was expecting 0.

```ruby
  1) Error:
NodeTest#test_node_value:
ArgumentError: wrong number of arguments (given 1, expected 0)
```

But.. Why is our linked list expecting 0? Because we don't have a constructor yet, let's do this.

`lib/node.rb`

```ruby
  attr_reader :value

  def initialize(value: nil)
    @value = value
  end
```
*Dont forget to add the `attr_reader` do make getter method*

If you run again your test it will pass, with this class now we have a node storing a value, if we build and install our gem now with `rake install` we'll see the fallowing response:

```ruby
linked_list 0.1.0 built to pkg/linked_list-0.1.0.gem.
linked_list (0.1.0) installed.
```

Now our gem are builded and installed on our system and we can open our IRB and do the fallowing commands:

```ruby
require "linked_list"

Node.new(value: 12)
Node.new(value: 10)
Node.new(value: 05)
```

And we're creating nodes, but this nodes isn't linked between each other, they're just in our memory storing values.

![Nodes not linked](https://s3.amazonaws.com/garagelabio/linked_list_ruby/nodes.png)

Now we need to create our links between each node, this links will be our `next node` and `previus node`, as you already know, let's start with test.

```ruby
  def test_next_node
    node2 = Node.new(value: 13, next_node: nil)
    node = Node.new(value: 12, next_node: node2)

    assert_equal node.next_node, node2
  end

  def test_previus_node
    node2 = Node.new(value: 13, previus_node: nil)
    node = Node.new(value: 12, previus_node: node2)

    assert_equal node.previus_node, node2
  end
```
*I gave myself the freedom os write both tests because each one is very similar*

Now just run your test and see the error:

```ruby
1) Error:
NodeTest#test_next_node:
ArgumentError: unknown keyword: next_node

 2) Error:
NodeTest#test_previus_node:
ArgumentError: unknown keyword: previus_node
```

Now, like the error is telling, we need to add `next_node` and `previus_node` to our node constructor, and `attr_accessor` for each one. (We need `attr_accessor` on each one because we'll need to set these previus and next nodes)

```ruby
class Node
  attr_reader :value
  attr_accessor :previus_node, :next_node

  def initialize(value: nil, previus_node: nil, next_node: nil)
    @value = value
    @previus_node = previus_node
    @next_node = next_node
  end
end
```

Our tests are passing now, to test our nodes with previus and next node, just run again "rake install" to install our builded gem and run IRB requiring "linked_list":

```ruby
2.4.1 :001 > require "linked_list"
 => true
2.4.1 :002 > node1 = Node.new(value: 12)
 => #<Node:0x00561c467f07c0 @value=12, @previus_node=nil, @next_node=nil>
2.4.1 :003 > node2 = Node.new(value: 10)
 => #<Node:0x00561c467dd1c0 @value=10, @previus_node=nil, @next_node=nil>
```

Now if we imagine, we'll have something like this:

![Two unlinked nodes](https://s3.amazonaws.com/garagelabio/linked_list_ruby/two_nodes.png)

Now with our `previus_node` and `next_node` we can link each node forming two interconnected nodes.

```ruby
2.4.1 :004 > node1.next_node = node2
 => #<Node:0x00561c467dd1c0 @value=10, @previus_node=nil, @next_node=nil>
2.4.1 :005 > node2.previus_node = node1
 => #<Node:0x00561c467f07c0 @value=12, @previus_node=nil, @next_node=#<Node:0x00561c467dd1c0 @value=10, @previus_node=#<Node:0x00561c467f07c0 ...>, @next_node=nil>>
```

Now if we run `node2.previus_node` ruby will return `node1` and if we run `node1.next_node` ruby will return `node2`.

![Two linked nodes](https://s3.amazonaws.com/garagelabio/linked_list_ruby/two_linked_nodes.png)

We've finished our Node logic, now we need to start working on List logic.

## 2.3 Creating LinkedList Class

We created all Node logic and we've free way to start creating all list logic, but you can ask me, what's logic?

* First Node, a method called `.first_node` to return the first node of the list.
* Last Node, a method called `.last_node` to return the last node of the list.
* Add Nodes, for this logic I want that our list have a method called `.add()`.
* Remove last node, create a `.pop` method, similar to [Array#pop](https://apidock.com/ruby/Array/pop) from Ruby.
* Remove first node, create a `.shift` method, similar to [Array#shift](https://apidock.com/ruby/Array/shift) from Ruby.
* Add first node, method called `.unshift`, similar again to [Array#unshift](https://apidock.com/ruby/Array/unshift).
* Print our list, I want a method that I can call with `.print` method and receive a graphical output on my terminal of my list.

So let's start defining our head of list (first node)

### 2.3.1 First Node of the List

Starting from the beginning, we need to store the first node of our list in a var of our `LinkedList` class, I'll name this variable `head` because this will be the "head" of our list.

Starting from the test, let's create a test that assert that we're storing the head of the node when creating our list.

Create file `test/list_test.rb`

```ruby
# frozen_string_literal: true

require "minitest/autorun"
require "node"

class ListTest < Minitest::Test
  def test_list_head
    node = Node.new(value: 123)
    list = List.new(head: node)

    assert_equal list.head, node
  end

  def test_list_head_setter
    node = Node.new(value: 123)
    list = List.new
    list.head = node

    assert_equal list.head, node
  end
end
```

Running our test now, we'll get the fallowing errors:

```ruby
  1) Error:
ListTest#test_list_head:
NameError: uninitialized constant ListTest::List
```

This error is simple, is telling that we don't have a class named List, to fix this just create List class.

`lib/list.rb`

```ruby
# frozen_string_literal: true

class List
end
```

*Dont forget to `require "list"` at our `lib/linked_list.rb`*

Running our tests again:

```ruby
  1) Error:
ListTest#test_list_head:
ArgumentError: wrong number of arguments (given 1, expected 0)
```

As we already know, this error is telling that we don't have a constructor on our class, so let's create it and test will pass.

*Note the use of `attr_accessor`, with this, the user don't need to pass the node when creating list, and can write `list.head = node`, satisfying the second test.*

```ruby
class List
  attr_accessor :head

  def initialize(head: nil)
    @head = head
  end
end
```

Now running our test, everything will pass, if you want you can install our gem and test this, everything will be working.

### 2.3.2 Adding Node

We need to create a `.add()` method as I had described before... Starting from the test, what we need to test? I think a good point to start is asserting that we are calling the `.add(value)` method and we're adding the new `node` at the end of the list (pointing the previus las node of th e list to this new last_node), and remember that if list is empty when we add node, this node will be the head node, on this test we've a lot of context but we're not working with rspec so, we will write a lot of test.

Starting from the beginning, we need to assert that inserting a node in a empty list, this node will be the head.

*Pay attention that this method will receive a value, not a node.*

```ruby
  def test_add_node_on_empty_list
    list = List.new
    list.add(12)

    assert_equal list.head.value, 12
  end
```

Now, we need to create the `.add(value)` method in the list and asserting that if list is blank, the head needs to be the node created from the passed `value`, this will be easy, is just check if head is nil, if is nil, create a node with value and stores at head.

```ruby
  def add(value)
    self.head = Node.new(value: value) if head.nil?
  end
```

Running our test, it will pass but we need to add nodes when our list is not empty.. and how to do that? First we need to pass between each node until find the last node (to find the last node would just check if `next_node` is `nil`), and when we find the last node we add the new node after it. Creating a method to return the last node it will look like this:

```ruby
  def last_node
    last_node = head

    until last_node.next_node.nil?
      last_node = last_node.next_node
    end

    item
  end
```

In a graphic view:

![Searching last node algorithm](https://s3.amazonaws.com/garagelabio/linked_list_ruby/search_last_node.png)


But, what's problem with that method of find the last node? It's that this method we're passing between each node, and this is a `O(N)` algorithm (Don't know what I'm talking about? [Read my post when I tech Big O with simple approach here](https://otaviovaladares.com/2018/02/08/big-o-notation/)), ok `O(N)` isn't a big problem with this example in image of four nodes but with a lot of nodes in our list, but with something like thousands of nodes it will be a problem.

To solve this problem, we have a good solution in this case, it's to create a `last_node` and everytime that we add a new node, we store it at `last_node` of the list, with this, when we add a new node, we check the current `last_node` of the list and stores the new node after this and set `last_node` of the list again, with this simple solution we will not have problems adding new nodes for any size of list. On this post we'll follow this approach.

Starting from the beginning..

*Note that we'll name last node of the list as `tail`.*

Creating the test and the method for the `tail`. (I'll skip some steps here because is the same as `head`).

```ruby
  def test_list_tail
    node = Node.new(value: 123)
    list = List.new(tail: node)

    assert_equal list.tail, node
  end

  def test_list_tail_setter
    node = Node.new(value: 123)
    list = List.new
    list.tail = node

    assert_equal list.tail, node
  end
```

And the code:

```ruby
  attr_accessor :head, :tail

  def initialize(head: nil, tail: nil)
    @head = head
    @tail = tail
  end
```

Backing to our `add` method, let's create the test.

```ruby
  def test_add_node
    list = List.new

    for number in 1..10 do
      list.add(number)
    end

    list.add(99)

    assert_equal list.tail.value, 99
  end
```

Running this test, obviusly it will fail, and now we need to complete our `.add` method, at this point we need to store the `tail` of our list. We have two context, one when list only have one node, this node will be the `head` and the `tail` of our list, and the second context when list have more than one node and we need to change the `tail` to the new node.

*I'll comment the fallowing code to be more clean on my explanation, but please, don't make your code with comments like these.*

```ruby
  def add(value)
    new_node = Node.new(value: value) # Creating our Node

    # Checking if is the first Node of the list
    self.head = new_node if head.nil?
    self.tail = new_node if tail.nil?

    # Making the new node the tail of the list

    # The previus node of the new node is the current tail
    new_node.previus_node = self.tail

    # The next node of the current tail is the new node
    self.tail.next_node = new_node

    self.tail = new_node # Set new node as tail
  end
```

*Note that we created the `new_node` to have the same node in all parts.*

![Adding new node](https://s3.amazonaws.com/garagelabio/linked_list_ruby/add_node_step_1+(1).png)

Run your test again and everything will be working, you can install your gem (as we done previusly) and test if you want.

### 2.3.3 .pop method

Ok, the most difficult part already gone, now we need to create simple methods to interact with our list, starting from `.pop` method. As I described at the beginning of chapter:

* Remove last node, create a `.pop` method, similar to [Array#pop](https://apidock.com/ruby/Array/pop) from Ruby.

This method is very similar to `.add` but we'll remove the last node, in Ruby we've a garbage collector so everything we need to do is to set de `previus_node` of tail as new list tail and set his `next_node` to nil.

*Implementing this in languages that doesn't have garbage collector like C you'll need to free the node that you're removing.*

![Removing Node](https://s3.amazonaws.com/garagelabio/linked_list_ruby/remove_node_pop.png)

Starting from Test, it just need to assert that after `.pop` the new tail is the expected.

```ruby
  def test_list_pop
    list = List.new

    for number in 1..10 do
      list.add(number)
    end

    list.pop

    assert_equal list.tail.value, 9
  end
```

Obviusly it will fail because we don't have the method, creating the method, we need that this method picks the `previus_node` of current list tail and sets it as new tail, after that point his `next_node` to nil, pretty simple isn't it?

*Again I'll use comments but I repeat is just for learning purpose.*

```ruby
  def pop
    old_tail = self.tail # Saving the old tail

    # Picking the previus node of the old_tail to be the new tail
    self.tail = old_tail.previus_node
    # Pointing the new tail to nil
    self.tail.next_node = nil

    # Setting the previus_node of old_tail to nil to completely isolate it
    old_tail.previus_node = nil
  end
```

*Note that on the last line of the method I set `old_tail.previus_node = nil` to completely isolate it from the rest of the list, facilitating for garbage collector of ruby eliminate it.*

Run our test again and everything will pass, signaling that our `.pop` is complete.

### 2.3.4 .shift method

This method will be very similar to `.pop` but as I've described we'll remove the head of the list.

* Remove first node, create a `.shift` method, similar to [Array#shift](https://apidock.com/ruby/Array/shift) from Ruby.

Everything we need to do is to set the `next_node` of the current head to be the new head!

![Removing Head](https://s3.amazonaws.com/garagelabio/linked_list_ruby/remove_head_shift.png)

Test:

Just need to assert that after `.shift` the new head is the expected.

```ruby
  def test_list_shift
    list = List.new

    for number in 1..10 do
      list.add(number)
    end

    list.shift

    assert_equal list.head.value, 2
  end
```

Now to make this test pass we need to create the `.shift` method. It is simple, is the same thing as the `.shift` just reversing the sides, look:

```ruby
  def shift
    old_head = self.head # Saving the old head

    # Picking the next node of the old_head to be the new head
    self.head = old_head.next_node

     # Pointing the new head previus_node to nil
    self.head.previus_node = nil

    # As I explained above, it's for garbage collector
    old_head.next_node = nil
  end
```

Everything is the same, if you understand one, automatically you understand another.

Run your test now and everything will pass.

### 2.3.4 .unshift method

As I described, now we've to create the `.unshift` method.

* Add first node, method called `.unshift`, similar again to [Array#unshift](https://apidock.com/ruby/Array/unshift).

This method is very similar to `.shift`, but instead of move the `head` we'll add a new `head` on the list.

How we do that? It's simple, we create the node that will be the new head and point his `next_node` to the old head, sets old head previus node  to the new node and after this set list head as the new node, pretty simple.

This test will be very simple, is just assert that after `.unshift(value)` our list head is this value.

```ruby
  def test_list_unshift
    list = List.new

    list.add(10)
    list.add(20)
    list.add(30)
    list.add(50)

    list.unshift(5)

    assert_equal list.head.value, 5
  end
```

Running tests, obviusly it will fail because we don't have `.unshift()` method created. Creating our method, it will be very similar, if you understand `.shift` and `.pop` you will easily understand this.

```ruby
  def unshift(value)
    new_head = Node.new(value: value)

    old_head = self.head

    old_head.previus_node = new_head
    new_head.next_node = old_head

    self.head = new_head
  end
```

If you run your test it will pass and if you want you can install your gem and test it by using.

*Note I'm not installing gem and testing each time we create a method because I'll test everything on the final of post, feel comfortable to install and test if you want.*

### 2.3.5 Printing our list

As I described in the beginning of the chapter.

* Print our list, I want a method that I can call with `.print` method and receive a graphical output on my terminal of my list.

This will be very funny and useful, it will help us use our list more easy and understand what is happening when working with the list.

At this method we have a lot of solutions and print designs possible, but for this test I'll use a very simple design, I want that when we call `list.print` the output looks like this:

```
12 <-> 23 <-> 45 <-> 12 <-> 56
```

Just each node pointing to the next/previus by an arrow.

Starting from the test, we need to create a test that assert the output of `.print` with a string of the expected output.

```ruby
  def test_list_print
    list = List.new

    list.add(10)
    list.add(20)
    list.add(30)
    list.add(50)

    assert_equal list.print, "10 <-> 20 <-> 30 <-> 50"
  end
```

*Pay attention that on head we dont have previus arrow, and at the tail we dont have next arrow.*

Running our test and failing it, we need to create our method, for this method we've a lot of ways of create this, one simple way that I think is good is to stores every node value at one array and after that join the array with `<->` and it will pass, the code will looks like this:

```ruby
  def print
    node = self.head
    output = []

    until node.nil?
      output << node.value

      node = node.next_node
    end

    output.join(" <-> ")
  end
```

This solution is very simple, we pass between each array, stores and after this, joins with arrows. If you run your test it will pass.

This solution is good and simple, I know that for this problem we've a lot of solutions and I'll love to learn more, if you have another idea, please comment, but for this post we'll continue with this.

*Another option is to include Enumerable module and pass between each node printing the arrows.*

### 2.3.6 Bonus Methods

Implementing a `.each` method, and after this including `Enumerable` module, we can have access to all power of `Enumerable` provide to us, it includes `.each`, `.map`, `.select` and much more.

*Don't know what I'm talking about? I've made a post/tutorial explaining it, check out here.*

Add `include Enumerable` at the beginning of our class, and implement it each method.

A nice test will be assert that our list is a `kind_of? Enumerable`

```ruby
  def test_list_each
    list = List.new

    assert_kind_of Enumerable, list
  end
```

And the method..

```ruby
  def each
    node = self.head

    until node.nil?
      yield node
      node = node.next_node
    end
  end
```

## 2.3 Manual Testing our Linked List

Ok, now everything we need to do is test, we already have `.print` method, so test, will be very funny.

Installing our gem with `rake install` and opening our IRB and requiring our gem with require "linked_list" we can start playing with our project.

```ruby
2.4.1 :001 > require "linked_list"
 => true
```

And if we create our list, I should have nil `head` and `tail`.

```ruby
2.4.1 :002 > l = List.new
 => #<List:0x005556db27b708 @head=nil, @tail=nil>
2.4.1 :003 > l.head
 => nil
2.4.1 :004 > l.tail
 => nil
```

And now if we add something it should be the `head` and `tail` of our node.

```ruby
2.4.1 :005 > l.add(23)
# ...
2.4.1 :006 > l.head.value
 => 23
2.4.1 :007 > l.tail.value
 => 23
```

Fine, is working as expected, now let's add a lot of nodes on our list.

```ruby
2.4.1 :008 > l.add(324)
2.4.1 :009 > l.add(10)
2.4.1 :010 > l.add(15)
2.4.1 :011 > l.add(20)
2.4.1 :012 > l.add(100)
```

Now we have 6 nodes on our list, and this is perfect to test our print method.

```ruby
2.4.1 :013 > l.print
 => "23 <-> 324 <-> 10 <-> 15 <-> 20 <-> 100"
```

Ok, print is working, now we need to test `.pop`, `.shift`, `.unshift()` and our bonus methods (Enumerable).

Testing on that order:

```ruby
2.4.1 :014 > l.pop
 => nil
2.4.1 :015 > l.print
 => "23 <-> 324 <-> 10 <-> 15 <-> 20"
2.4.1 :016 > l.shift
 => nil
2.4.1 :017 > l.print
 => "324 <-> 10 <-> 15 <-> 20"
2.4.1 :018 > l.unshift(10000)
# ...
2.4.1 :019 > l.print
 => "10000 <-> 324 <-> 10 <-> 15 <-> 20"

# Testing Enumerable

2.4.1 :020 > l.each do |node|
2.4.1 :021 >   puts node.value
2.4.1 :022?> end
10000
324
10
15
20
```

We finished our tests, feel comfortable to test anything you want.

## 3.0 Finishing

Now we got our list finished and working and I hope that you have learned a lot with this post. I love to implement data structures because I think that we can learn a lot doing this simple exercises, on the next post I'll talk about ordered datasets and sorting and search algorithms and I think it will be very funny. (depending on when you are reading this, probably I already writed that post, you can check [here](https://otaviovaladares.com))

Remembering, that the code of this is list you can find on my github [here](https://github.com/OtavioHenrique/linked_list)

And if you have any question please ask anywhere, you can comment this post, PM me on [twitter](https://twitter.com/valadaresotavio) or send me an email (otaviopvaladares@gmail.com), I'll love it!

Don't forget to follow me on my [twitter](https://twitter.com/ValadaresOtavio)

Have a great studies!
