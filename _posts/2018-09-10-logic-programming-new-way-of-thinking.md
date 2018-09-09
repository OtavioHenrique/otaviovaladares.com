---
layout: post
title: "Logic Programming"
subtitle: "Programming by another point of view"
date: 2018-08-12 13:10:00
author: "Octos"
header-img: "img/in-post/logic-programming/the-thinker.jpg"
header-mask: 0.5
catalog: true
multilingual: true
tags:
- Programming Languages
- Paradigm of Programming
- Logic Programming
- Prolog
- Theory
---

Let’s talk about logic programming, I think everybody who completed the college or study computer science by yourself (like me), already have heard about logic programming, what it’s exactly? When talking about programming we have a lot of paradigms of programming languages and logic is one of them, but unfortunately isn’t very popular, to be sincere Prolog is most used by academia. But don’t worry learn logic programming can be good to expand your knowledge and see the things from other perspectives, understanding a new paradigm of programming is a new way of thinking.

On this post, I'll talk about what I've liked on Prolog and how to get started.

Warning:

* I'm not an expert of Prolog
* This post doesn't have advanced content
* Every example of this post will be written in Prolog
* This post has a strong theoretical content, if you don't like, feel free to jump to "First Steps" paragraph.

"The inception of logic is tied with that of scientific thinking. Logic provides a precise language for the explicit expression of one’s goals, knowledge, and assumptions. Logic provides the foundation for deducing consequences from premises; for studying the truth or falsity of statements given the truth or falsity of other statements; fir establishing the consistency of one’s claims; and for verifying the validity of one’s arguments.” - Sterling, 1986 The art of Prolog.

## What is logic?

I think the key to understanding logic programming is not your background in computer systems, the key is your knowledge of philosophy, you need to know and understand what is the Aristotelian logic, I’m not a philosopher so I’ll try to explain from my point of view on the simplest way, not going deep on advanced topics, only a resume.

Let’s go back to our high school and talk about mid 350 B.C, Aristotle the Ancient Greek philosopher developed something named term logic aka traditional logic or Aristotelian logic.

### Aristotle

*Warning: If you want only the hands-on part, stop here and go to "First Steps"*

Aristotle's logical work is collected in the six texts that are collectively known as the Organon. Two of these texts in particular, namely the Prior Analytics and De Interpretatione, contain the heart of Aristotle's treatment of judgments and formal inference, and it is principally this part of Aristotle's works that is about term logic.

![Aristotle](https://s3.amazonaws.com/garagelabio/logic_programming/aristotle.jpeg)

The fundamental assumption behind the theory is that propositions are composed of two terms, where terms is a part of speech representing something, and that the reasoning process is in turn built from propositions.

Propositions need to follow the [Three Laws of Thought](https://oregonstate.edu/instruct/phl201/modules/Philosophers/Aristotle/aristotle_laws_of_thought.html)

* Law of Identity, A is A. Everything is the same as itself.
* The Law of Noncontradiction, NOT ( A and not A) - Nothing can both exist and not exist at the same time and in the same respect, or no statement can be both true and false.
* Law of Excluded Middle, EITHER (A or not A) - Something either exists or doesn’t exists, or every statement is either true or false.

This reasoning process is often called syllogism (“conclusion, inference”), that is when you apply deductive reasoning to arrive at a conclusion based on two or more propositions that are asserted or assumed to be true.

The syllogism is basically one conclusion based on prepositions.

When talking about syllogism, the most common example is the one given by Socrates.

All men are mortal.
Socrates is a man.
Therefore, Socrates is mortal.

Each syllogism consists of three parts:

Major premise
Minor premise
Conclusion

In the rest of this text you'll learn that prolog can do that, based on fact and rules.

This is only a simple resume of what is logic and syllogism, if you felt interested, you can continue studying by yourself.

“Logic is an instrument for advancing knowledge"

## What is Logic Programming?

Most programmers at day to day work deal with imperative languages, the main characteristic of imperative languages is the way that you change the programs state giving a statement, step by step (How to).

Programs written in a logic programming languages, computation is dealing with relations rather than with single-valued functions, these relations are defined by a set of rules about some problem domain and these rules are written by the programmer, this concept leads us to one of the main characteristics of logic programming:

"Logic programming is composed of two things, the logic, and the control."

The logic component is the definition of the problem while the control is more like the way to get the solution, the rules.

We can define a logic programming algorithm by the fallowing formula:

*Algorithm = Logic + Control*

*Where "Logic" represents a logic program and "Control" represents different theorem-proving strategies.*

---

Logic programming is a type of programming paradigm which is largely based on formal logic, that's more like "What is", with you asking the computer for answers, it is known as "declarative programming".

So, in a simple conclusion, logic programming is much like telling the system the problem, the rules based on formal logic, the facts, and asking for the answer (declarative) than giving a step by step instructions to solve the problem.

## What is Prolog?

It’s hard to talk about logic programming without talk about Prolog. Prolog is a logic programming language based on first-order logic and formal logic, designed in 1970 by Alain Colmerauer. The program logic is expressed in terms of relations, represented as facts and rules. A computation is initiated by running a query over these relations. Today, I think Prolog is the most popular logic programming language.

The name is a French abbreviation for "Programming in Logic".

## First Steps

All the pillars of logic programming inherit from logic (that we studied above), terms and statements.

We've three basic statements: *Facts*, *Rules* and *Queries* and a single data structure called *Logical Term*

### Facts

The simplest kind of statement in Prolog is called *fact*, and we'll start our study talking about it.

Fact is basically the relation the objects hold between each other:

```erlang
mother(elizabeth, charles).
```

This is the most basic example of the fact, it holds the relation between Elizabeth and Charles an tell that this relation is a mother.

We say *atoms* to refer to the name of the things that the predicate is telling the relations, on this example, Elizabeth and Charles are both *atoms*.

You can also speak *predicate*, *fact* and *predicate* means the same.

*Note that both predicate and atoms start with a lowercase letter, the reason we'll talk soon.*

A set of facts can describe a situation or a series of relations, which will be used by Prolog to execute computations.

In Prolog, you need to use dot to say that you finished your statement, don't forget the dot.

With facts we can define simple relations, for example, a family tree, this example is considered the "Hello World program of Prolog", and that's what we're going to do now.

For this example, we'll use the left side of the Britsh royal family tree. (We'll use only the left side because the entire tree is too big, and would escape the main goal of this text)

![Left side of royal family tree](https://s3.amazonaws.com/garagelabio/logic_programming/royal_family_simple+(1).png)

Ok, take a look at this tree, how many facts can we define? I think that we can define a lot of predicates, since parental relations till dates like birth or marriage.

To keep this example as simple as possible, let's define only three, *mother*, *father*, *male/female*.

```erlang
female(elizabeth).
male(philip).

father(philip, charles).
mother(elizabeth, charles).

male(charles).

father(charles, william).
mother(diana, william).

male(william).

father(charles, harry).
mother(diana, harry).

male(harry).

father(william, george).
mother(catherine, george).

male(george).

father(william, charlotte).
mother(catherine, charlotte).

female(charlotte).

father(william, louis).
mother(catherine, louis).

male(louis).
```

A little bit big, right? Now imagine if we wrote all the facts about the royal family, like dates, marriage, etc.

Now would be fabulous if we can consult these facts, would it?

### Queries

Queries is the second form of statement in logic programs, they are the way of retrieve information from facts. We can also speak *goal*.

A query asks Prolog what's the relationship between objects, for example, we want to know if Charles is the father of William, just ask for Prolog:

```erlang
?- father(charles, william).
```

*During this text everytime that you see `?-` before a statement is because we're working on query context.*

And Prolog will return `true`, and it will return `false` with we ask something that's not true:

```erlang
?- father(diana, william).
```

*One nice trick is to think like we're asking a question, and Prolog will respond based on facts that it already knows, this is possible because we're working with declarative programming.*

#### The prolog interpreter

When working with simple problems like this, we usually load our prolog file into Prolog interpreter to start querying, assuming that you're using [SWI-prlog](http://www.swi-prolog.org/), you've two ways:

* Start prolog shell using `swipl` and after this load prolog file writing `consult('path/to/file.pl')`.
* Already loads `swipl` with your database, using `-s` option, for example: `swipl -s royal_family.pl`

Some useful commands that can help you in your Prolog journey:

* `halt.` closes your interpreter.
* `listing.` shows facts and rules loaded.
* `help.` obviusly.
* `assert(fact).` adds fact to your base.
* `retract(fact).` removes fact of your base.

Don't feel scared, this is like any REPL of an interpreted language.

This aspect of Prolog can look strange and make some people feel confused at the beginning but don't worry.

Syntactically both queries and facts can look the same, but you can easily differ by the context.

### The logical variables

Unlike *facts* or *queries* variables isn't a statement, but we need to talk about too.

Variables in Prolog is very different from other languages, they don't store a specific value at is memory, so, the first step is to stop thinking that variables is to store values, not here. Variables in logic programming stand for an unspecified entity and the interpreter will try to instantiate the variables for us respecting the facts previously defined. Let's understand it better by example, imagine that you want to know who is William's children?

```erlang
?- father(william, X).
```

*Remember, this is query context, not fact context.*

The first thing we note at this query is that for the first time, we're using uppercase letter at `X`, this is because variables in Prolog start with an uppercase letter. This query now has a variable, what's it means?

If we run the previous query, you'll get the following result:

```erlang
X = george
```

Ok, Prolog has solved it for you, they found that William is the father of George, now he's telling you that, but... William is father of more children and Prolog has returned only one, simple, when Prolog is promoting something to you and your variable can have more than one result, you can press dot for end your query (signaling to Prolog that this answer is ok for you), or you press semicolon to go to the next answer (this doesn't work when Prolog found only one possible answer).

```erlang
?- father(william, X).
X = george ;
X = charlotte ;
X = louis.
```

Prolog gives to us all possibles value for X.

This can look very hard, but it isn't when talking about *rules* this can look more simple. One way to better understand what we're asking to Prolog is to read the query like:

"Does there exist an X such that William is the father of X?"

### Rules

Rules are the third and the most important statement of Prolog.

Rules are the way of define patterns/rules to Prolog make decisions based on facts previously defined. Let's think about, on our base we've all the information about father, moms, and sex. We're humans and if our intelligence we can make conclusions based on these facts, Prolog can make conclusions too, but we need to teach how to make these conclusions.

For example, we can define who is grandparents of each other, right? What is the rule for define grandparent?

![Grandparents Rules](https://s3.amazonaws.com/garagelabio/logic_programming/grandparents_rule.png)

The rule is simple, for someone to be your grandfather, he needs to be the father of your father, right? How do we define that in Prolog? Simple, we create a rule, it will be called `grandfather`.

```erlang
grandfather(X, Y) :-
father(X, Z),
father(Z, Y).
```

We can read this rule like this:

*For all `X`,`Y`. `X` is the grandfather of `Y`, if `X` is the father of `Z` and `Z` is the father of `Y`.*

Now if we reload our base at Prolog interpreter, we can use our new rule! Let's try it:

```erlang
?- grandfather(X, louis).
X = charles.
```

Nice, we have found Louis grandfather and it's Charles. How our rule behaved when we asked who is the grandfather of Louis?

First, of step, Louis on our rule is the `Y`, so every `Y` becomes louis.

```erlang
grandfather(X, luis) :-
father(X, Z),
father(Z, louis).
```

The second step, solve `father(Z, louis).` and found that `Z = william`.

```erlang
grandfather(X, luis) :-
father(X, william),
father(william, louis).
```

The last step, now fallowing your defined rule, we need to solve `father(X, william).` to find who is the grandfather of Louis, the answer is `X = charles`, and we have found our goal.

```erlang
grandfather(charles, luis) :-
father(charles, william),
father(william, louis).
```

It's not that hard, right? The most interesting thing about this rule is that it can find grandchildren too, just replace X by the name of grandfather and find all grandchildren:

```erlang
?- grandfather(philip, Y).
Y = william,
Y = harry
```

And obviously, we can make assertions without variables:

```erlang
?- grandfather(philip, william).
true
```

You can use one rule to define another rule, for example, you can use `grandfather/2` rule, inside the `great-grandfather` rule.

```erlang
greatgrandfather(X, Y) :-
father(X, Z),
grandfather(Z, Y).
```

With this power, you actually can build nice things using logic programming.

---

Now we know the three states of logic programming, and solved the most basic exercise of Prolog, the "family tree", this problem as I said, is like the "hello world" of Prolog.

With our current knowledge, we can rewrite the most famous example of a syllogism that we discussed above in Prolog:

    All men are mortal.
    Socrates is a man.
    Therefore, Socrates is mortal.

```erlang
mortal(X) :- man(X).
man(socrates).

...

? - mortal(socrates).
true
```

## Solving Four color theorem

The four color theorem is a nice example of something that would use a lot of lines to be completed in most common programming languages like Java or Python that in Prolog consume only a few lines of code.

The four color theorem is a theorem of mathematics. It says that in any plane surface with regions in it (people think of them as maps), the regions can be colored with no more than four colors. Two regions that have a common border must not get the same color. They are called adjacent (next to each other) if they share a segment of the border, not just a point. [Interested? Read more about it here.](https://simple.wikipedia.org/wiki/Four_color_theorem)

On this example, I'll paint the map of an imaginary country that has only five states, I think that's enough for this exercise.

![Map](https://s3.amazonaws.com/garagelabio/logic_programming/country_png.png)

To get started, the best thing to do is to define the facts, we can start defining the four colors of our map. What colors do you like? Choose four.

```erlang
color(black).
color(byzantine).
color(sapphire_blue).
color(screamin green).
```

With the colors of our map defined, it's time to define the rule of the problem, two adjacent states can't have the same color. We will need to use the `=/=` not equal operator.

We'll create a rule that says "if two states are neighbors they don't have the same color"

```erlang
adjacent(state1Color, state2Color) :-
color(state1Color), color(state2Color),
state1Color =/= state2Color.
```

And now map all states that are adjacent to each other, defining a new rule called `country/5`.

```erlang
country(A, B, C, D, E) :-
adjacent(A, B), adjacent(A, C), adjacent(A, D),
adjacent(B, E), adjacent(B, C),
adjacent(C, D), adjacent(C, E),
adjacent(D, E).
```

*Note, if we already defined `adjacent(A, C)` we don't need to define `adjacent(C,A)`.*

If you open your prolog shell now and call your country rule, you'll get the output with the colors needed.

```erlang
?- country(A, B, C, D, E).
A = black,
B = D, D = byzantine,
C = sapphire_blue,
E = screamin_green
```

If we paint our map again based on Prolog output

![Colored Map](https://s3.amazonaws.com/garagelabio/logic_programming/COLORED_MAP.png)

And the problem is done, you can replicate this problem to every map and we'll have a correct answer.

One courious think about this problem, is that it was the first major theorem to be proved using a computer.z

## Final Notes

#### Warnings while loading family tree on interpreter

When you load the royal family prolog file at interpreter you'll get a bunch of warnings, like this:

```
Clauses of male/1 are not together in the source-file
Earlier definition at /home/otavio/Documents/prolog/royal_family.pl:2
Current predicate: mother/2
Use :- discontiguous male/1. to suppress this message
```

The problem is that our base is not grouped by facts, and Prolog encourages it. To solve this warning just group our facts.

#### Execute prolog without installing it

Just enter on this site https://swish.swi-prolog.org/ and start using, on the left side your base and at right bottom corner your queries, just click on "run" and the magic begins.

Philosophy reference

https://en.wikipedia.org/wiki/Term_logic
https://en.wikipedia.org/wiki/Syllogism#cite_note-11
https://pt.wikipedia.org/wiki/L%C3%B3gica_aristot%C3%A9lica
https://en.wikipedia.org/wiki/Law_of_noncontradiction
https://en.wikipedia.org/wiki/Law_of_excluded_middle
https://en.wikipedia.org/wiki/Law_of_identity
https://oregonstate.edu/instruct/phl201/modules/Philosophers/Aristotle/aristotle_syllogisms.html
https://en.wikipedia.org/wiki/Law_of_thought#The_three_traditional_laws

Logic Programming Reference

file:///home/otavio/Documents/prolog-palazzo.pdf
https://en.wikipedia.org/wiki/Logic_programming#Logic_and_control
https://dev.to/matchilling/introduction-to-logic-programming-with-prolog-cdh
http://sarabander.github.io/sicp/html/4_002e4.xhtml#g_t4_002e4
https://en.wikipedia.org/wiki/Imperative_programming