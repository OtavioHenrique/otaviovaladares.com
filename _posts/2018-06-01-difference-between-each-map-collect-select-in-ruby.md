---
layout:       post
title:        "each, map, select in ruby? Understanding Enumerable"
subtitle:     "stop guessing forever"
date:         2018-06-02 04:04:00
author:       "Octos"
header-img:   "img/in-post/enumerable/banner.jpeg"
header-mask:  0.5
catalog:      true
multilingual: true
tags:
    - Tutorial
    - Beginners
    - Ruby
---

When you're starting with ruby programming, a lot of people (and me too), usually have questions about the difference between ruby `.map`, `.select`, `.collect`, `.each` and many others enumerables.

## Each

Each executes for each element the logic passed in block.

For example, if you want to `print` each element of one array using what we usually do in another languages with `for` loops, it will look like this:

```ruby
places = ["restaurant", "mall", "park", "theater"]

for i in 0..places.size
puts places[i]
end

# => restaurant
# => mall
# => park
# => theater
```

Ok, this works but is not readable, it's not a natural language, and in ruby, we love to prioritize it, with `.each` it will be much more lovely.

```ruby
places.each do |place|
  puts place
end

# => restaurant
# => mall
# => park
# => theater
```

Or if you want or are working on IRB you can use inline syntax.

```ruby
places.each { |place| puts place }
```

### Another Example

```ruby
numbers = [1, 2, 3, 4, 5]
numbers.each { |number| puts number * 2 }

# => 2
# => 4
# => 6
# => 8
# => 10
```

Pay attention, `each` doesn't alter the array.

*Note, on last example we multiply each element by 2, but if we print, the array is not multiplied*

```ruby
puts numbers

# => 1
# => 2
# => 3
# => 4
# => 5
```

*Note too, if we try to save each block at one variable it doesn't work, because it will return the same array*

```ruby
a = numbers.each { |number| puts number * 2 }
# ..
puts a
# => [1, 2, 3, 4, 5]
```

## Map

Different from each, the `.map` is very powerful because it returns an array with the results of the block.

*Note, the final output is an array*

```ruby
numbers.map { |number| number * 2 }

# => [2, 4, 6, 8, 10]
```

More understandable example:

*On this example we apply `.upcase` on each element, after this we've an array `places_upcase` with each place in upcase.*

```ruby
places = ['restaurant', 'mall', 'park', 'theater']
places_upcase = places.map { |place| place.upcase }
# => ["RESTAURANT", "MALL", "PARK", "THEATER"]

places_upecase
# => ["RESTAURANT", "MALL", "PARK", "THEATER"]
```

## Collect

Collect is the same as `.map` it's just an alias.

## Select

Also Known As "Filter" in other languagens, `select` executes the passed expression for each element of array, if is true, it adds to the final response array, for example:

*On this code we use `select` to return an array with all numbers greater than 3.*

```ruby
numbers = [1, 2, 3, 4, 5]

numbers.select { |number| number > 3 }
# => [4, 5]
```

Another good example is to use `select` to get all odd numbers of one array.

```ruby
odd_numbers = numbers.select { |number| number.odd? }

odd_numbers
# => [1, 3, 5]
```

# Behind the scnenes

`map`, `select`, `collect` isn't all, you have over 50 methods in `Enumerable` mixin.

Now you can ask me, what is this Enumerable mixin? It's simple.

`Enumerable` it's a ruby module, it provides collection classes with several traversal and searching methods, a lot of methods, as you can see:

```ruby
Enumerable.instance_methods
# => [:to_a, :to_h, :include?, :find, :entries, :sort, :sort_by, :grep, :grep_v, :count, :detect, :find_index, :find_all, :select, :reject, :collect, :map, :flat_map, :collect_concat, :inject, :reduce, :partition, :group_by, :first, :all?, :any?, :one?, :none?, :min, :max, :minmax, :min_by, :max_by, :minmax_by, :member?, :each_with_index, :reverse_each, :each_entry, :each_slice, :each_cons, :each_with_object, :zip, :take, :take_while, :drop, :drop_while, :cycle, :chunk, :slice_before, :slice_after, :slice_when, :chunk_while, :lazy]

Enumerable.class
# => Module
```

Wow, it's awesome, isn't it?

And for being a module, you can include it on your class and have access to this collection of powerful methods. But, how to include it? It's not just add `include Enumerable ` to your class, you need to create an `each` method in your class that yields every element of collection.

*As described in ruby documentation: "The class must provide a method each, which yields successive members of the collection."*

Let's use a simple example to apply this, a class named `OddNumbers`.

*The class have a `each` method that yields three odd numbers.*

```ruby
class OddNumbers
  include Enumerable

  def each
    yield 1
    yield 3
    yield 5
  end
end
```

After do that, you already can see all `Enumerable` methods in your class.

```ruby
OddNumbers.instance_methods
# => [:each, :to_a, :to_h, :include?, :find, :entries, :sort, :sort_by, :grep, :grep_v, :count, :detect, :find_index, :find_all, :select, :reject, :collect, :map, :flat_map, :collect_concat, :inject, :reduce, :partition, :group_by, :first, :all?, :any?, :one?, :none?, :min, :max, :minmax, :min_by, :max_by, :minmax_by, :member?, :each_with_index, :reverse_each, :each_entry, :each_slice, :each_cons, :each_with_object, :zip, :take, :take_while, :drop, :drop_while, :cycle, :chunk, :slice_before, :slice_after, :slice_when, :chunk_while, :lazy, :instance_of?, :public_send, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :private_methods, :kind_of?, :instance_variables, :tap, :method, :public_method, :singleton_method, :is_a?, :extend, :define_singleton_method, :to_enum, :enum_for, :<=>, :===, :=~, :!~, :eql?, :respond_to?, :freeze, :inspect, :display, :object_id, :send, :to_s, :nil?, :hash, :class, :singleton_class, :clone, :dup, :itself, :taint, :tainted?, :untaint, :untrust, :trust, :untrusted?, :methods, :protected_methods, :frozen?, :public_methods, :singleton_methods, :!, :==, :!=, :__send__, :equal?, :instance_eval, :instance_exec, :__id__]
```

And you can see it as included module too

```ruby
OddNumbers.included_modules
# => [Enumerable, Kernel]
```

*Note, include `Enumerable` module without provide `each` method in your class, it won't works.*

And this is why ruby's `Array` and `Hash` have this methods, on their code, they include `Enumerable`.
```ruby
Array.included_modules
# => [Enumerable, Kernel]
Hash.included_modules
# => [Enumerable, Kernel]
```

## Conclusion

A lot of people when are starting with ruby programming think all these methods it's all the same (and it's normal!) but they aren't, when you understand each one, not only each/select/map but a lot of others of Enumerable module, you have great power in your fingers, obviously nobody will memorize all methods of this module, but you can search on documentation when you need something that smeels like an Enumerable method.

[I pretty recommend that you read the documentation of Enumerable](https://ruby-doc.org/core-2.5.1/Enumerable.html)

If you have any question that I can help you, please ask! Send email (otaviopvaladares@gmail.com), pm me on my [twitter](https://twitter.com/opvaladares) or comment this post!

[Don't forget to fallow me on twitter](https://twitter.com/ValadaresOtavio)

*This post was originally written in portuguese by me, [here](https://medium.com/collabcode/diferença-entre-map-collect-select-e-each-no-ruby-4d8dc853711f)*
