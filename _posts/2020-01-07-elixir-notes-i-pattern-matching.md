---
layout:       post
title:        "Elixir Notes I - Pattern Matching"
subtitle:     "An introduction to pattern matching in Elixir"
date:         2020-01-07 01:20:00
author:       "Otavio H. P. Valadares"
header-img:   "img/in-post/elixir-notes-i-pattern-matching/bg.jpg"
header-mask:  0.5
catalog: true
multilingual: true
tags:
  - Elixir
  - Beginners
  - Tutorial
---

[Background art by Dmitry Burmak](https://www.artstation.com/artwork/R4WwE)

When you came across Elixir, one of the first things that you’ll hear about is the famous “Pattern Matching” and Elixir’s match operator, this is not a coincidence since many things in Elixir/Erlang are built around it.

No wonder, a lot of courses and books explain pattern matching in the first chapters, [“Programming Elixir” by Dave Thomas](https://pragprog.com/book/elixir16/programming-elixir-1-6) is probably the most famous book reference and explains it in the first few chapters, I recommend it. In this post I’ll explain what pattern matching is based on what I studied in recent months, I hope I can explain clearly.

## Understanding it

When you learn your first programming language(in most cases), you learn to use the equal sign in another way, different from what you learned at your school when younger.

You learn that you use an equal sign to assign values to variables:
```ruby
irb(main):001:0> number = 10
=> 10
```

Ok, Elixir can make variable assignment like that:
```elixir
iex(1)> x = 1
1
```

But the main difference behind Elixir equal sign is on the next example:

```elixir
iex(1)> x = 1
1
iex(2)> x = 2
2
iex(3)> 2 = x
2
iex(4)> 3 = x
** (MatchError) no match of right hand side value: 2
```

Did you realize something strange here? Let’s try the same in Ruby.

```ruby
irb(main):001:0> x = 1
=> 1
irb(main):002:0> x = 2
=> 2
irb(main):003:0> 2 = x
SyntaxError ((irb):3: syntax error, unexpected '=', expecting end-of-input)
2 = x
  ^
```

It occurs because Elixir doesn’t assign values to a variable using equal sign like other languages, its equal sign is more like what you learned in algebra, from the definition:

*“Shows that what is on the left of the sign is equal in value or amount to what is on the right of the sign.”*

![Equal sign Elixir](https://garagelabio.s3.amazonaws.com/elixir-notes-i-pattern-matching/equal_sign.png)

When we typed `2 = x` for elixir is like as we typed `2 = 2`, it’s true, so it evaluated 2. In ruby and most imperative languages, it tries to assign value 2 two Integer 2, so it fails. At the next line of our Elixir example, we tried to `3 = x` and it raised `MatchError` telling that it can’t match 3 to 2, and that’s true.

Let's summarize the algorithm at this point:

Elixir tries to check if both values are equal, if true, it’s a valid expression, if not, it raises a `MatchError`, but if we have a variable on the left side it will match everything biding the value of the right side on the left side.

Ok, at this time you may be thinking “Ok, it’s different from the most common equal sign in programming, but it can’t change my daily work”, keep calm and let’s study more complex uses of it.

We already learned the concept of matching, but when the “pattern” goes on? We can use patterns to make more complex matches between the left and right side of equal, let’s see some examples to get started.

```elixir
iex(1)> [a, b, c] = [1, 2, 3]
[1, 2, 3]
iex(2)> a
1
iex(3)> b
2
iex(4)> c
3
```

The language always will try to make the right side equal to the left side, on the previous example, it sees a list [1, 2, 3] on the right side and another of [a, b, c] on the right side, to make both equal it just need to bind the values of right side into left side.

Elixir always will try to equalize both sides, based on its patterns.

```elixir
iex(1)> [a, b, [c, d]] = [1, 2, [20, [30, 40]]]
[1, 2, [20, [30, 40]]]
iex(2)> a
1
iex(3)> b
2
iex(4)> c
20
iex(5)> d
[30, 40]
```

![Elixir complex pattern matching](https://garagelabio.s3.amazonaws.com/elixir-notes-i-pattern-matching/pattern_matching_2.png)

This process of treat equals sign as an assertion of equivalence and bind values of the right side into left side variables based on patterns is called pattern matching.

Elixir provides two useful operators to help us with pattern matching, one of these is the underscore operator, you can use it when you don’t want to assign a value during pattern matching, and it will ignore any value that is assigned and will help your code be more readable, every time that someone looks at these, will know that the code doesn’t care about what is assigned to underscore.

```elixir
iex(1)> [a, _] = [1, 2]
[1, 2]
iex(2)> a
1
iex(3)> _
** (CompileError) iex:4: invalid use of _. "_" represents a value to be ignored in a pattern and cannot be used in expressions
```

Lists have a problem that we don’t know previously its size, if we have a function that can return lists of differents size pattern matching will be hard, we don’t know how to match it on the left side. Fortunately, Elixir provides us an option to match against the head and tail of the list and it is the other operator that can help us with pattern matching.

```elixir
iex(4)> [head | tail] = [10, 20, 30, 40, 50]
[10, 20, 30, 40, 50]
iex(5)> head
10
iex(6)> tail
[20, 30, 40, 50]
```

Elixir always will assign list’ head to the variable on the left side of the pipe, and the rest (also known as tail) to the variable on the right side of the pipe.

![Elixir pipe operator separating head and tail from list](https://garagelabio.s3.amazonaws.com/elixir-notes-i-pattern-matching/pattern_matching_3_pipe.png)

## Function Responses

Pattern matching is also useful to check the return of functions, but how it works? In Elixir is very common to functions return what we call “tagged tuples”, that’s nothing more than a tuple with the first item being an atom describing functions return status. The pattern is simple when the function was successful, its returns `{:ok, response}` and when an error occurred `{:error, response}`.

As our example, let’s analyze the responses of [Poison](https://github.com/devinus/poison), the most popular Elixir JSON library.

```elixir
iex(10)> Poison.Parser.parse(~s({"name": "Otavio Valadares", "age": 22}), %{})
{:ok, %{"age" => 22, "name" => "Otavio Valadares"}}
iex(11)> Poison.Parser.parse(~s({"name": "Otavio Valadares", "age": }), %{})
{:error, {:invalid, "}", 36}}
```

This convention, allow us to make pattern matching against function returns, we can match against `:ok` and `:error` tags.

```elixir
defmodule Example do
  def parse(json) do
    case Poison.Parser.parse(json) do
      {:ok, result} ->
        IO.inspect(result)
      {:error, error} ->
        IO.inspect(error)
      _ ->
        "Unexpected Error"
    end
  end
end

Example.parse("{\"name\": \"Otavio Valadares\", \"age\": 22}")
# => %{"age" => 22, "name" => "Otavio Valadares"}

Example.parse("{\"name\": \"Otavio Valadares\", \"age\":}")
# => {:invalid, "}", 35}
```

At this example, we use pattern matching on case statement based on `Poison.Parser.parse` the response, when it returns a tuple with first atom `:ok` we bind the response (second element) to result in variable and prints it, and we do the same when it responds with`:error` tuple.

![Pattern matching against function result](https://garagelabio.s3.amazonaws.com/elixir-notes-i-pattern-matching/pattern_matching_result.png)

Most libraries in Elixir follow this pattern on responses, so get used to working with it. The previous example shows just one of several ways that we can handle it, and obviously, it’s not the best solution, but a good way to beginners get started.

## Multi-Clause Functions

We learned the basics of pattern matching and now it’s time to talk about the icing on the cake, Elixir’s pattern match can also control our software flow, choosing what function calls based on args and arity, this is the craziest thing that pattern matching can give to us.

If we declare two functions and send a message, we can choose the function that will be called.

As our example about it, [we will use an example based on this video that I pretty recommend it](https://www.youtube.com/watch?v=V_4-Iswp3II&t=1135s).

Suppose that we have a module called Geometry that will calculate the area of different shapes, disregarding object oriented programming, what would you consider doing? I think the most common solution will create a function that receives an argument that is the shape to be calculated, and its numbers. This solution probably will need a lot of ifs or case statement to check what shape is and do the math.

With Elixir this problem has a different and cleaner way to be solved using what is called “Multi-clause Functions”, we can define multiple functions with the same name but each one with a different function clauses, Elixir will try each clause until it finds one that matches (using pattern matching).

Let’s begin with our example:

```elixir
defmodule Geometry do
  def area({:square, a}) when is_number(r) do
    {:ok, a * a}
  end

  def area({:circle, r}) when is_number(r) do
    {:ok, r * :math.pi}
  end
end
```

We defined our module `Geometry` and inside it defined two functions, we have both functions named `area` but one receiving a tuple with `{:square, a}` and other one expecting `{:circle, r}`. Let’s try to use our module:

```
IO.inspect Geometry.area({:square, 10})
# => {:ok, 100}
IO.inspect Geometry.area({:circle, 5})
# => {:ok, 15.707963267948966}
```

Elixir will choose which function execute based on what was sent to function, it will try to pattern match against each one if it successfully matched, this function will be executed.

![Function pattern matching](https://garagelabio.s3.amazonaws.com/elixir-notes-i-pattern-matching/pattern_match_functions.png)

And of course, if we call passing a function clause that no one functions handle, it will raise a `FunctionClauseError` error:

```elixir
IO.inspect Geometry.area({:rectangle, 4, 8})
# (FunctionClauseError) no function clause matching in Geometry.area/1
```

To avoid this kind of error, we can define a function that handles everything to returns error when something unexpected was sent, but it’s important to note that function need to be placed at the end of the module because Elixir will execute the first function that matches against.

```elixir
def area(unknown) do
  {:error, {:unknown_shape, unknown}}
end
```

Multi-clause functions with pattern matching are another tool that elixir brings to us that can be used to control our software flow, it is powerful when understood can be easily used to solve a variety of problems.

## Conclusions

As we saw in this post, pattern matching is powerful and lives in Elixir soul, it’s impossible to look at any project and don’t see pattern matching being used everywhere,  you can use pattern matching to unpack any data structure and control your software flow, that’s why pattern matching matters. What we saw in this post was an introduction necessary to get started in my opinion.

## Final thought

If you have any question that I can help you, please ask! Send an email (otaviopvaladares@gmail.com), pm me on my [Twitter](https://twitter.com/valadaresotavio) or comment on this post!

### Reference

https://elixir-lang.org/getting-started/pattern-matching.html
https://steemit.com/utopian-io/@tensor/introduction-to-elixir---pattern-matching-and-control-flow---part-three
https://inquisitivedeveloper.com/lwm-elixir-40/
https://subscription.packtpub.com/book/application_development/9781788472678/1/ch01lvl1sec16/control-flow
https://medium.com/podium-engineering/using-control-flow-in-elixir-to-improve-discoverability-21341b27d851
https://elixirschool.com/en/lessons/basics/control-structures/
