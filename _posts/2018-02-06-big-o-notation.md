---
layout:       post
title:        "Understanding basic of Big O notation"
subtitle:     "without a lot of mathematic"
date:         2018-02-08 01:36:00
author:       "Octos"
header-img:   "img/in-post/bigo/algorithm.png"
header-mask:  0.5
catalog:      true
multilingual: true
tags:
    - Algorithms
    - Tutorial
    - Beginners
---

*Big O* notation is a famous topic of computer science, but this topic still afraid a lot of people. Some programmers don't have a complete cs degree and never studied this, and those who have studied almost always didn't understood very well.

On this text I will explain what really is *Big O*, and what you really need to know for don't feel ashamed with your friends. 

I'll explain all the basic of *Big O* notation with examples written in Ruby.

**This is not a complete guide of *Big O* notation, if you want to know everything about, please, search for a complete reference or book**

# 1. Introducing 

## This text is not recommended for

- People who have solid knowledge of *Big O* notation.
- People who want to know everything about *Big O*

## What I'll explain on this post

- What is *Big O*
- The most common notations
- Simple examples
- Simple Benchmarks

## 1.1 First of all, what is Big O notation? 

*Big O* notation is the language and metric we use when talking about growth rates and efficiency of algorithms. Can be used to describe the behavior of and algorithm in terms of the growth in the number of operations as the number of elements processed increases, with this technique we can evaluate how our code will behaves with a lot of data or operations.

*If you want a long exaplanation you can read the big o article on wikpedia [here](https://en.wikipedia.org/wiki/Big_O_notation)*

## 1.2 Why is it important? I'll never use it.

A lot of methods or data structures of every programming language are described using <i>Big O</i>, like binary search that in ruby can be called `array#bsearch`.

First of all, if you always work with small sizes of data and operations, maybe you'll never use it. But it's good to understand the basic concepts of algorithms, because can help you to make better design decisions some times and construct better algorithms.

Nowadays on the big data century, tons of data and operations are reality. You'll probably need to design an algorithm thinking on his Time or Space complexity. 

It's better to know because a lot of methods of your favorite programming language have difference times, with *Big O* you can understand better the performance of each one.

*And remember, a lot of big tech companys like Google, Facebook and Amazon asks for big o answers on their interview process.*

# 2. Understanding 

## 2.1 Visualizing the complexity

On *Big O* notation we have a lot of possibilities of notations, and we don't have a fixed list of runtimes but the most commom is:

| Notation   | Short Description | Denomination |
|------------|-------------| ------------ |
| O(1)       | The best solution possible | Constant |
| O(log n)   | Almost good, binary search is the most famous algorith O(log n) | Logarithmic |
| O(N)       | The previus example, when you walk through each data. "ok" solution | Linear |
| O(n log n) | Not bad solution. Most famous algorithm is Merge Sort. | nlogn |
| O(N^2)      | Horrible, you can see the example bellow | Quadratic |
| O(2^n)      | Horrible, most famous algorithm is quicksort             | Exponential |
| O(N!)      | The most horrible solution, factorial is when you test each possible solution | Factorial |

![Big O complexy chart](https://s3.amazonaws.com/garagelabio/big-o/bigo.png)

## 2.2 Time or Space?

We can use *Big O* to describe both time or space, but what it means? Simple, one algorithm that use *O(N)* runtime, will use *O(N)* space too, but it's not that easy, some algorithms can be *O(N)* in time complexity but *O(N^2)* in space.

## 2.3 Time for what?

Data structures or algorithms can have multiple *Big O* times for multiple actions, for example: An Array have *O(1)* time to access, but *O(n)* to search, all depends of the scope of our code.

[On this site you can find all the time complexity of each famous algorithm and data structure](http://bigocheatsheet.com/)

## 2.4 Big O, Big Omega, Big Theta?

 On this text I'll not talk about *Big Omega* and *Big Theta* because of two reasons:

- This is a simple guide and I don't want to dive into deep concepts of computer science.

- The industry tends to use *Big O* notation.


## 2.5 Presenting our problem

On your life you never thought that some actions of your day spend a lot of time to be done, and you can make simple actions to reduce the time spent? Let's use a simple example, dictionary(physical edittion, not google), how much time do you need to find a word? Depend.

*"The Second Edition of the 20-volume Oxford English Dictionary contains full entries for 171,476 words in current use, and 47,156 obsolete words. To this may be added around 9,500 derivative words included as subentries."*

#### Code

*For learning purpose on all examples I'll use an array of numbers not strings.*

Let's create the array that I'll use on all next examples. This array will be our dictionary, but instead of words(strings) we'll use numbers. The word "zoo", will be the last word of our dictionary and will be represented as number 1000000. As you can see below:

```ruby
dictionary = []

0.upto(1000000) do |number|
  dictionary << number
end
```

Now, we have an array with one million of numbers between 1 and 1000000. Let's assume that this array of number is our dictionary and the 1000000 on last slot it's our word "zoo".

### 2.5.1 First Solution O(N)

Considering that this dicionary is in alphabetical order, how much time you'll spend to find the word "zoo" if you go through each word? (Consider constant to flip pages)

Yes, to find "zoo" you'll spend a lot of time because you'll need to go through all words. These scenarios when we walk through each data, on *Big O* notation we call *O(n)*. Let's see the code.

#### Code

Now let's assume that we're going though each word until we find the word.

```ruby
def o_n(dictionary)
  dictionary.each do |word|
    word if word == 1001
  end
end

o_n(dictionary)
```

This is a very common and as I discribed previusly, is not a bad solution, but not good to. In terms of dificulty to understand is not hard too, every example of algorithm *O(n)* we have iterators on lists.

### 2.5.2 Second Solution *O(1)*

Let's assume that now you already knows what is the location of word zoo on dictionary, every time that you need to search for it you only need to open the dictionary on the page and just read. You don't need to find for the word, you already knows its location.

#### Code

*O(1)* is usually refered as an algorithm that computer already knows the location of information, and only need to access it.

```ruby
def o_one(dictionary)
  dictionary[1000000]
end

o_one(dictionary)
```

*O(1)* is always the best solution, and you probably use it every day, the most commond usage of it is with [Hashes](https://en.wikipedia.org/wiki/Hash_function).

I think everybody here knows, but Ruby already have Hash implemented.

#### Code

```ruby
hash = {
  color: "red"
}

hash[:color]

# => "red"
```

### 2.5.3 Third Solution *O(log n)*

Some people have a lot of difficult to undersand the concept of *O(log N)* algorithm, but I hope that this example with dictionary history can help you undersand:

Let's assume that you don't know where the word is located, but you know that if you search each word (*O(N)* will spend a lot of time, and you don't know where the word is located,what will you do? You could start opening the dictionary on a random page and search if the words of this page come before or after the word that you're looking for, if is after you open on another page after this page, and look again... and repeat it until you find the word that you're searching for.

If you already studied algorithms you probably noted that I described one of the most famous algorithms of all time, the [binary search](https://en.wikipedia.org/wiki/Binary_search_algorithm) and that is the most famous example of *O(log n)* algorithm.

Let's see the code.

#### Code

For this example I'll use a simple binary_search implementation (Not recursive)

```ruby
def binary_search(dictionary, number)
  length = dictionary.length - 1
  low = 0

  while low <= length
    mid = (low + length) / 2

    return dictionary[mid] if dictionary[mid] == number

    if dictionary[mid] < number
      low = mid + 1
    else
      length = mid - 1
    end
  end

  "Number/Word not found"
end

puts binary_search(dictionary, 1000000)

```

To indentify an algorithm that is *O(log n)* you can fallow two simple steps:

- The choice of the next element on which to perform some action is one of several possibilities, and only one will need to be chosen.
- The elements on which the action is performed are digits of n
*You can check more [here](https://stackoverflow.com/questions/2307283/what-does-olog-n-mean-exactly)*

Binary tree is a perfect exemple of this steps.

Like many languages ruby already have binary search built-in as array methods as Array#bsearch as you can se below:

```ruby
array = [1,2,3,4,5,6,7,8,9,10]

array.bsearch {|x| x >= 9 }

# => 9 
```

You can read more [here](http://ruby-doc.org/core-2.2.0/Array.html#method-i-bsearch)

[If you want to understand why it name is *O(log n)* click here](https://hackernoon.com/what-does-the-time-complexity-o-log-n-actually-mean-45f94bb5bfbf)

*If you think that this is not enough at references you find better links to better understand, I'll not explain a lot of maths because it's a text for beginners.*

### 2.5.4 Fourth Solution *O(N^2)*

Imagine that you're searching word by word like on *O(N)* but for some reason you need to make your search again, this is the *O(N^2)*. With this example applied on our history of the dictionary is hard to understand but with code is more easily.

The trick is basic and everybody can start thinking that *O(N^2)* is basically a loop inside a loop.

#### Code

```ruby
def o_n2(dictionary)
  dictionary.each do |word|
    dictionary.each do |word2|
      word if word == 1000000
    end
  end
end

o_n2(dictionary)
```

Don't worry, this is a very common case too, almost every person already have done something like that, but this code don't have a good performance for large data. The best way is think if you can reduce this algorithm to *O(log n)*.

### 2.5.5 Fifth Solution *O(n log n)*

This is the most hard definition for me, and I admit that I spend hours searching on internet, because the definitions that I've found didn't satisfy me (And don't have much answers on internet).

The simplest explanation that I've found is:

*O(n log n) is basically you running n times an action that costs O(log n)*

This is obvius but, makes sense. Let's examine:

In the binary tree, inserting one node (in a balanced tree) is *O(log n)* time. Inserting a list of n elements into that tree will be *O(n log n)* time.

*O(n log n)* code can be often the result of an otimization of quadratic algorithm. The most famous example of this type is the merge sort algorithm. 

For this definition won't have short history, I definitely didn't think in anything that makes sense on our history with dictionary.

*If you think that this is not enough at references you find better links to better understand, I'll not explain a lot of maths because it's a text for beginners.*

### 2.5.6 Sixth Solution *O(2^N)*

Exponencial runtime, this is a very hard definition, like o(n log n). This type of problem is when the number of instructions growth exponentially, it's common to found it on recursive algorithms, or some search algorithms. 

*"When you have a recursive function that makes multiple calls, the run time will be O(2^n)"*

If you search on internet you'll probably see a lot of people talking that fibonnaci recursive solution is a good example:

```ruby
def fibonacci(number)
  return number if number <= 1

  fibonacci(number - 2) + fibonacci(number - 1)
end

puts fibonacci(10)
```

For find a O(2^N) you can fallow simple rules:

O(2^N) denotes an algorithm whose growth doubles with each additon to the input data set. (Exponential growth)

For this definition won't have short history, too, sorry guys.

### 2.5.7 Seventh Solution *O(N!)*

The factorial solution, is very horrible. But I think there's no example in our history of finding words in dictionary, and I don't want to dive inside a lot of math in this beginners guide.

*O(N!) is every time that you calculate all permutation of a given array of size n.*

The most common example of this notation is that solving salesman problem via brute-force(testing every solution possible). (A lot of sites such as wikpedia uses this example).

#### Code

In ruby we have a method that returns all permutations of a given array.

```ruby
array = [1,2,3]

array.permutation(3).to_a

# => [[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1]]
```
This is actually *O(N!)*

# 3. Some Benchmarking

Let's examine some benchmark of algorithms of each example and compare, for better understanding.

On this section I'll use the same implementations of algorithms from previus chapter but using ruby [Benchmarking](http://ruby-doc.org/stdlib-2.0.0/libdoc/benchmark/rdoc/Benchmark.html) module to compare the execution times.

## 3.1 *O(1)* vs *O(N)*

For this example we'll use the same dictionary array created on previus chapter with one million of data.

```ruby
0.upto(1000000) do |number|
  dictionary << number
end
```

So if we run the algorithms created in th above chapter:

```ruby
Benchmark.bm { |bench| bench.report("o(1)") { o_one(dictionary) } }
Benchmark.bm { |bench| bench.report("o(n)") { o_n(dictionary) } }

#       user     system      total        real
# o(1)  0.000000   0.000000   0.000000 (  0.000005)
#       user     system      total        real
# o(n)  0.060000   0.000000   0.060000 (  0.052239)

```

A very low differente, and that's is why *O(N)* on the graph of complexy is not very bad, with one million of data he just needed `0.052239s`, but and if we growth your dataset to almost 15 million, the *O(N)* solution will be good? Let's see:

```ruby
#       user     system      total        real
# o(1)  0.000000   0.000000   0.000000 (  0.000004)
#       user     system      total        real
# o(n)  0.750000   0.000000   0.750000 (  0.741515)
```

The time of *O(N)* solution increased 12.5x more than the array of one million, and that's is very wrong if we're talking about scalable systems on the big data century.

## 3.1 *O(N)* vs *O(N^2)*

First of all, let's create again an array but this time with lil bit less data:

```ruby
data = []

0.upto(20000) do |number|
  data << number
end
```

Yes, twenty thousand is good enought to this example, because *O(N^2)* is very bad, and if you growth you data, you'll problaby get 100% cpu usage on ruby process.

Let's examine the runtime of previus *O(N)* and *O(N^2)* algorithms of our dictionary example using benchmark module and the data array created:

```ruby
Benchmark.bm { |bench| bench.report("o(n)") { o_n(data) } }
Benchmark.bm { |bench| bench.report("o(n^2)") { o_n2(data) } }

#       user     system      total        real
# o(n)  0.010000   0.000000   0.010000 (  0.000963)
#       user     system      total        real
# o(n^2) 19.550000   0.000000  19.550000 ( 19.560149)
```

*Obviusly that this number will vary*

This difference is pretty high, and that is where *Big O* came. Remember, on Big O we're talking about the "behavior of an algorithm in terms of the growth in the number of operations as the number of elements" as I talked on previus chapter. 

Ok, twenty thousand of data is pretty high from some people, and if we try an array of five thousand, is it enought?

```ruby
       user     system      total        real
o(n)  0.000000   0.000000   0.000000 (  0.000252)
       user     system      total        real
o(n^2)  1.330000   0.000000   1.330000 (  1.329480)
```

If you're thinking: "The differente is so much smaller, I don't need to optimizate this code." 

You're pretty wrong depending of your situation, because if we get the differente between and execution of each algorithm we have `1.329228s` of difference, and it can be an eternety for your costumer, if you consider that your client wait `1.329228s` three times each day, we're stealing `27.9s` per week of your costumer, and we're only working with twenty thounsand of data, I'm sure that a lot of people work with a lot of more.

So, in this case of an *O(n^2)* I pretty recomend that you try to reduce this for an *O(log n)*

# 4. Finishing

I'll not benchmark all algorithms and data structures, but I pretty recommend that you study and test others one. Specially the *O(log n)*

## 4.1 What's next? 

If you want to be a rockstar at *Big O* just keep studying, reading a lot.

You can read:

- [Introduction to Algorithms, Third Edition](https://mitpress.mit.edu/books/introduction-algorithms)
- [Cracking the Coding Interview](https://www.amazon.com/Cracking-Coding-Interview-Gayle-McDowell/dp/0984782850/ref=as_li_ss_tl?ie=UTF8&linkCode=sl1&tag=careercup-ctciwebsite-20&linkId=173f3d8878a1d7f0d131a85fbfc9f67f) 

This last books, have one chapter 100% dedicated to *Big O* notation, and I pretty recommend for those who wants to study more deep.

You can fallow the reference links bellow and study by yourself.

# 5. Reference

- [How to benchmark ruby](http://mitrev.net/ruby/2015/08/28/benchmarking-ruby/)
- [Whats does O(log n) means](https://stackoverflow.com/questions/2307283/what-does-olog-n-mean-exactly)
- [Good docs from one university](http://www.dca.fee.unicamp.br/~ting/Courses/ea869/faq1.html#item7)
- [How to calculate Big O](https://stackoverflow.com/questions/3255/big-o-how-do-you-calculate-approximate-it)
- [Linear time vs Quadratic Time](https://stackoverflow.com/questions/18023576/linear-time-v-s-quadratic-time)
- [How to calculate complexity of recursive functions](https://stackoverflow.com/questions/13467674/determining-complexity-for-recursive-functions-big-o-notation)
- [O(log n), why is that name?](https://hackernoon.com/what-does-the-time-complexity-o-log-n-actually-mean-45f94bb5bfbf)
- [Little bit more about O(log n)](https://stackoverflow.com/questions/5201013/on-log-log-n-time-complexity)
- [And more about O(log n)](https://stackoverflow.com/questions/36318159/big-on-log-n-explanation)
- [Time complexity wikpedia](https://en.wikipedia.org/wiki/Time_complexity)
- [Good video about](https://www.youtube.com/watch?v=MyeV2_tGqvw)