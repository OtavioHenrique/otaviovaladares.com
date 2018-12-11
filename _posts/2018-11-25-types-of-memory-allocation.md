---
layout:       post
title:        "Types of Memory Allocation, a brief view"
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

I've many criticisms about some ways that people usually teaches C programming, one of these is that is very difficult to find someone that explain the difference between each type of variable and allocation, they usually only say "that is a variable and use it" or "look, this is a malloc(), start using it", on this text I'll try to show the difference between each type, for those who didn't learn it learn and for those who learned it remember it!

## Types of Allocation

#### Automatic Memory Allocation

Automatic memory allocation corresponds to the automatic variables, also known as "stack" memory, it's memory allocated at runtime when you enter into a new scope, each time that a function is called its auto variables are pushed to stack.

*Worried because you don't know what is stack, don't panic, this will be explained on the next topic.*

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

The lifetime of automatic variables is only during the execution of its scope/block, when the function/block is called the variable is pushed to stack and start your life, after the end of the function/block it is popped off of the stack and doesn't exists anymore.

```c
void anyThing() {
    int autoVariable = 10;

    autoVariable += 10;
}

int main() {
    anyThing();
    /* The variable autoVariable doesn't existis here, it only exists inside its function*/

    return 0;
}
```

*It can looks obvious to majority of programmers but I think that few ask themselves about why it occurs.*

Summarize: Every variable declared without any keyword(or `auto` keyword) at any function will be considered "auto" variables, and will be automatic allocated during runtime of the software at memory using the program's stack, this kind of variable life time is only during the execution of the function block or scope, after this, the variable is pushed out of the stack.

#### Static Memory Allocation

Static variables is other type of variables and memory allocation, the major difference is that it preserve its value, they aren't created/destroyed each time that your program calls a function. It corresponds to file scope variables and local static variables, their lifetime is the lifetime of the program.

To declare a static variable, the only thing that you need to do is to use `static` keyword, like the fallowing example:

```c
static int age = 18;
```

This characteristic can be used to do interesting things, the variable is not destroyed when your software get out of the scope, so you can use it to count how many times one function is called, this is the most popular example of static variables:

```c
#include <stdio.h>


void count() {
    static int counter = 0;
    counter++;
    printf("Counter = %d\n", counter);
}

int main() {
    count();
    count();
    count();
    count();

    return 0;
}
```

The scope of a static variable is the scope of the function, the only difference is that they aren't destroyed on the final of the function call, if you try to access a static variable outside of its scope you'll get compilation error.

They are allocated on compile time that fixes their addresses and sizes (so, the executable of the software already have the location and size of these variables), the name "static" is because they do not vary in location or size during the lifetime of the program (because the compiler already fixed it at executable).

It's important to know that they aren't stored at stack like automatic allocation, or at the heap (like the dynamic allocation that we'll see bellow), this kind of variable are stored in the data segment of the programs address space if the are initialized or the BSS segment if they aren't initialized.

*Worried because you don't know what is BSS/Data Segment/Stack/Heap? Don't panic, this will be explained on the next topic.*

The compiler fixes the address and size of the static variable at compile time, so a static variable can only be initialized using constant literals, because the compiler needs to know the value of the variable, you can't initialize a static variable with a returned value of a function for example, the fallowing program will generates a compile error:

```c
#include <stdio.h>


int initializer(void) {
    return 50;
}

int main() {
    static int i = initializer();
    printf(" value of i = %d", i);
    getchar();

    return 0;
}
```

Some people can think that static variables and global variables are the same thing, but they aren't, they have different scopes, you can't access static variable of one function `int count()` inside the function `int sum()`, while the global variable can be accessed in any function. But some question may appear, like, if we put a static variable on the global scope, it will be the same as a global variable? and the answer is no. Let's take a look:

```c
#include <stdio.h>


static int counter x;
int counter y;

int main () {
  // anything
}
```

On this example we have two global scoped variables but the `x` is of static type, now you can ask me, what's the difference? `x` will have static linkage (file scope), the variable is only accessible in the current file, while the `y` has a external linkage and you can refer to this variable if you declare it with `extern` in another file, both variables follow the same rules to be stored (as I described before).

Summarize: Static variables are statically allocated and their size and addresses are fixed at compiler time, the major difference is that it preserve its value, they aren't created/destroyed each time that your program call a function, they have function scope and are stored at data/BSS segment.

#### Dynamic Memory Allocation

The famous dynamic memory allocation that scares a bunch of people at the college, it isn't a brain surgery and I'll prove to you, dynamic memory allocation can be very interesting and fun if you really understand it from the beginning.

I think one of the most common doubts among students is "Why did I need to use dynamic memory allocation, I don't need it!!"

https://en.wikipedia.org/wiki/C_dynamic_memory_allocation
https://www.geeksforgeeks.org/what-is-dynamic-memory-allocation/
https://www.programiz.com/c-programming/c-dynamic-memory-allocation

## Links

https://en.wikipedia.org/wiki/C_dynamic_memory_allocation
https://stackoverflow.com/questions/8385322/difference-between-static-memory-allocation-and-dynamic-memory-allocation
https://stackoverflow.com/questions/8385322/difference-between-static-memory-allocation-and-dynamic-memory-allocation
https://en.wikipedia.org/wiki/C_dynamic_memory_allocation#Usage_example
http://mpatrol.sourceforge.net/doc/Static-memory-allocations.html
https://www.tutorialspoint.com/cprogramming/c_arrays.htm
https://stackoverflow.com/questions/11720079/how-can-i-see-the-size-of-files-and-directories-in-linux
https://www.geeksforgeeks.org/static-variables-in-c/
https://www.geeksforgeeks.org/memory-layout-of-c-program/
https://github.com/EdmundKorley/libfree/wiki/memory-layout-of-c-programs
https://www.programiz.com/c-programming/c-arrays
http://fractallambda.com/2014/10/30/Dynamic-Static-and-Automatic-memory.html
https://www.includehelp.com/c/difference-between-automatic-auto-and-static-variables.aspx
https://www.geeksforgeeks.org/memory-layout-of-c-program/
http://mpatrol.sourceforge.net/doc/Static-memory-allocations.html
https://en.wikipedia.org/wiki/Static_variable
https://stackoverflow.com/questions/959889/difference-between-global-and-static-global
https://stackoverflow.com/questions/44954524/difference-between-global-and-static-variable-in-c
https://stackoverflow.com/questions/2271902/static-vs-global
http://codingstreet.com/c-basic-questions/what-is-difference-between-global-and-static-variables-in-c/
http://faculty.cs.niu.edu/~freedman/241/241notes/241var2.htm
https://overiq.com/c-programming-101/local-global-and-static-variables-in-c/


## Final thought

If you have any question that I can help you, please ask! Send an email (otaviopvaladares@gmail.com), pm me on my [Twitter](https://twitter.com/ValadaresOtavio) or comment on this post!
