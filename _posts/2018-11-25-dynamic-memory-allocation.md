---
layout:       post
title:        "Dynamic Memory Allocation, a brief view"
subtitle:     "Understanding the basics"
date:         2018-11-25 01:10:00
author:       "Octos"
header-img:   "img/in-post/pointers/pointers-brief-view.jpg"
header-mask:  0.5
catalog:      true
multilingual: true
tags:
  - C
  - Computers Architecture
  - Tutorial
  - Beginners
---

A few months ago, I started a series of posts about important topics of C programming and computer architecture, the first post was about pointers and you can check [here](https://otaviovaladares.com/2018/08/12/pointer-a-brief-view/), on this post I'll talk about dynamic memory allocation, I'll show what problem it solves and how to use.

## Types of Allocation

### Automatic Memory Allocation

Automatic memory allocation corresponds to the automatic variables, also known as "stack" memory, it's memory allocated at runtime when you enter into a new scope, each time that a function is called its auto variables are pushed to stack.
What it really means? An auto variable is every variable that you declare and don't use any keyword (like `static`), simplifying, variables that you declare inside your `main` function or inside any function can be considered auto variables, therefore, automatic memory allocation.

```c
#include <stdio.h>

int main() {
    int a = 1;
    int b = 2;

    printf("Sum: %d", a+b);

    return 0;
}
```

On the previous example, both variables `a` and `b` are auto variables. It's important to remember that in C you have the keyword `auto` to specify this kind of variables, but by default every variable inside a block/scope it's a `auto` variable, so you don't need to use this keyword.

This example is the same as the previous:

```c
#include <stdio.h>

int main() {
    auto int a = 1;
    auto int b = 2;

    printf("Sum: %d", a+b);

    return 0;
}

```

Variables declared inside scopes are `auto` too:

```c
if (a < b) {
    int c = 10;
}
```

The lifetime of automatic variables is only during the execution of its scope/block, when the function/block is called the variable is pushed to stack and start your life, after the end of the function/block it is pushed out of the stack and doesn't exists anymore.

```
void anyThing() {
    int autoVariable = 10;

    autoVariable += 10;
}

int main() {
    anyThing();
    /* The variable autoVariable doesn't existis here, it only exists inside its function*/
}
```

It can looks obvious to majority of programmers but I think that few ask themselves about why it occurs.

Summarize of auto variables: Every variable declared without any keyword(or `auto` keyword) at any function will be considered "auto" variables, and will be automatic allocated during runtime of the software at memory using the program's stack, this kind of variable life time is only during the execution of the function block or scope, after this, the variable is pushed out of the stack.

### Static Memory Allocation



## Final thought

If you have any question that I can help you, please ask! Send an email (otaviopvaladares@gmail.com), pm me on my [Twitter](https://twitter.com/ValadaresOtavio) or comment on this post!
