---
layout:       post
title:        "Types of Memory Allocation, a brief view"
subtitle:     "Understanding the particularities of each type"
date:         2018-11-25 01:10:00
author:       "Octos"
header-img:   "img/in-post/types-of-memory-allocation/amazonia.jpg"
header-mask:  0.5
catalog:      true
multilingual: true
tags:
  - C
  - Computers Architecture
  - Tutorial
  - Beginners
---

A few months ago, I started a series of posts about important topics of C programming and computer architecture, the first post was about pointers and you can check [here](https://otaviovaladares.com/2018/08/12/pointer-a-brief-view/), on this post I'll talk about types of memory allocation.

I've many criticisms about some ways that people usually teach C programming, one of these is that is very difficult to find someone that explains the difference between each type of variable and allocation, they usually only say "that is a variable and use it" or "look, this is a malloc(), start using it", on this text I'll try to show the difference between each type, for those who didn't learn it learn and for those who learned it remember it!

This is a topic that I see few authors writing about, and I think few people have learned about this while studying C programming, on this post(and on the next talking about C memory layout), I hope I help people understand better this part of computer programs.

## Types of Allocation

### Automatic Memory Allocation

#### How to use

Automatic memory allocation corresponds to the automatic variables, also known as "stack" memory, it's memory allocated at runtime when you enter into a new scope, each time that a function is called its auto variables are pushed to the stack.

*Worried because you don't know what is the stack, don't panic, this will be explained on the next post.*

What it really means? An auto variable is every variable that you declare and doesn't use any keyword (like `static`), simplifying, variables that you declare inside your `main` function or inside any function can be considered auto variables, therefore, automatic memory allocation.

```c
#include <stdio.h>


int main() {
    int a = 1;
    int b = 2;

    printf("Sum: %d", a+b);

    return 0;
}
```

In the previous example, both variables `a` and `b` are auto variables. It's important to remember that in C you have the keyword `auto` to specify this kind of variables, but by default every variable inside a block/scope it's an `auto` variable, so you don't need to use this keyword.

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

The lifetime of automatic variables is only during the execution of its scope/block when the function/block is called the variable is pushed to the stack and start your life, after the end of the function/block it is popped off of the stack and doesn't exist anymore.

```c
void anyThing() {
    int autoVariable = 10;

    autoVariable += 10;
}

int main() {
    anyThing();
    /* The variable autoVariable doesn't existis here */
    /* it only exists inside its function*/

    return 0;
}
```

*It can look obvious to the majority of programmers but I think that few ask themselves about why it occurs.*

#### Summarize

Every variable declared without any keyword(or `auto` keyword) at any function will be considered "auto" variables, and will be automatically allocated during runtime of the software at memory using the program's stack, this kind of variable lifetime is only during the execution of the function block or scope, after this, the variable is pushed out of the stack.

### Static Memory Allocation

Static variables are another type of variables and memory allocation, the major difference is that they preserve its value, they aren't created/destroyed each time that your program calls a function. It corresponds to file scope variables and local static variables, their lifetime is the lifetime of the program.

##### How to use

To declare a static variable, the only thing that you need to do is to use `static` keyword, as the following example:

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

The scope of a static variable is the scope of the function, the only difference is that they aren't destroyed on the final of the function call, if you try to access a static variable outside of its scope you'll get a compilation error.

They are allocated on compile time that fixes their addresses and sizes (so, the executable of the software already have the location and size of these variables), the name "static" is because they do not vary in location or size during the lifetime of the program (because the compiler already fixed it at executable).

It's important to know that they aren't stored at stack like automatic allocation, or at the heap (like the dynamic allocation that we'll see below), this kind of variable is stored in the data segment of the programs address space if they are initialized or the BSS segment if they aren't initialized.

*Worried because you don't know what is BSS/Data Segment/Stack/Heap? Don't panic, this will be explained on the next post.*

The compiler fixes the address and size of the static variable at compile time, so a static variable can only be initialized using constant literals, because the compiler needs to know the value of the variable, you can't initialize a static variable with a returned value of a function for example, the following program will generate a compile error:

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

Some people can think that static variables and global variables are the same things, but they aren't, they have different scopes, you can't access static variable of one function `int count()` inside the function `int sum()`, while the global variable can be accessed in any function. But some question may appear, like, if we put a static variable on the global scope, it will be the same as a global variable? and the answer is no. Let's take a look:

```c
#include <stdio.h>


static int counter x;
int counter y;

int main () {
  // anything
}
```

On this example, we have two global scoped variables but the `x` is of the static type, now you can ask me, what's the difference? `x` will have static linkage (file scope), the variable is only accessible in the current file, while the `y` has an external linkage and you can refer to this variable if you declare it with `extern` in another file, both variables follow the same rules to be stored (as I described before).

#### Summarize

Static variables are statically allocated and their size and addresses are fixed at compile time, the major difference is that it preserves its value, they aren't created/destroyed each time that your program calls a function, they have function scope and are stored at data/BSS segment.

### Dynamic Memory Allocation

The famous dynamic memory(aka DMA) allocation that scares a bunch of people at the college, it isn't brain surgery and I'll prove to you, dynamic memory allocation can be very interesting and fun if you really understand it from the beginning.

*Reinforcing the importance of pointers to dynamic memory allocation, I suggest that you have a solid knowledge of pointers, you can read any tutorial on the internet, or [the one that I have here, on my blog](https://otaviovaladares.com/2018/08/12/pointer-a-brief-view/).*

Different from previous examples, dynamic memory is allocated on the heap, that I'll explain in details on next post.

To dynamic allocate memory in C language(this part its different from C++), you need to include `stdlib.h` to have access to functions that C provides to dynamic allocation, you can check all functions that you'll gain access [here](https://www.tutorialspoint.com/c_standard_library/stdlib_h.htm) but on this post we'll only take a look at only four of them:

```c
void *malloc(size_t size);
void free(void *ptr);
void *calloc(size_t nmemb, size_t size);
void *realloc(void *ptr, size_t size);
```

#### malloc()

Let's start with `void *malloc(size_t size)` probably one of the most famous functions of computer science. It allocates `n` bytes of memory and returns as a pointer, it's important to be careful because `n` needs to be the exact number of bytes that you need to store your data, for example, if we want to store a simple information of type `int` as we remember we'll need 8 bytes, so, you pass 4 to the `malloc()` and it'll return a pointer to the first byte of this block of memory:

```c
int *number = malloc(4);
```

Now your variable `number` is a pointer to a block of 4 bits of memory, the necessary to store an integer:

```c
#include <stdio.h>

#include <stdlib.h>


int main () {
    int *number = malloc(4);

    *number = 1234;

    printf("%d", *number);
}
```

*If you try to store a number that has more than 4 bits you'll get an overflow error.*

You probably are asking yourself if you'll need to know the size of each data that you'll store, and fortunately the answer is no, you don't have to know all the data size, we have a function that returns the data size and is called `sizeof()`, to use it is very simple, just pass the variable type(or a variable of the type) that you want. Combining it with the previous `malloc()` you'll have great power in your hands.


```c
#include <stdio.h>

#include <stdlib.h>


int main () {
    int *number = malloc(sizeof(int));

    *number = 1234;

    printf("%d", *number);
}
```

If you need to create an array dynamically just multiply the size by the number of items of your array, for example, if you want an array of 10 ints, just do that:

```c
int *numbers = malloc(sizeof(int)* 10);
```

#### free()

Another important function from `stdlib.h` is `free()`, it's so important to your memory management and if you forgot to use `free()` you'll have a memory leak. Simplifying, C programming language don't have a garbage collector, it means that all memory that is dynamically allocated don't get destroyed by the language in runtime, and you need to manage memory use by yourself while coding, if you allocate a new memory using `malloc()` when you stop using this variable you SHOULD free it.

```c
#include <stdio.h>

#include <stdlib.h>


int main () {
    int *numbers = malloc(sizeof(int) * 10);

    for(int i = 0; i < 10; i++) {
        numbers[i] = i + 10;
        printf("%d\n", numbers[i]);
    }


    free(numbers);
}
```

#### calloc() and realloc()

We have also two more functions to discuss here, `calloc()` and `realloc()`, they are very simple if you understand the concept of `malloc()` let's start with `calloc()`.

`calloc()` is equal to `malloc()` with two differences: `calloc()` it initializes all values of your block with 0 while `malloc()` initializes each block with garbage values, and it takes two arguments, the number of blocks to be allocated and size of each block, with this, if you want to allocate an array of 10 ints, you don't need to multiply 10 by the size of each block, you just need to pass ten as argument.

```c
int *numbers = calloc(10, sizeof(int));
```

`realloc()` as the name says you can re-allocate a memory previously allocated using `malloc()` in other words, if you have an array previous allocated with 5 ints using `malloc()` and you want to resize it to 10 ints, you can use `realloc()`. To use it, you only need to pass the previous pointer and the new size. Using it, you'll resize the allocated space on the heap.

```c
int *numbers = malloc(sizeof(int)* 5);

int *newNumbers = realloc(numbers, sizeof(int)* 10);
```

I'll not enter in details of how to use `malloc()`, `realloc()`, `calloc()` and free because isn't the objective of the text, on a future post I'll only write about how to use this functions with mastery.

#### Why use DMA?

I think one of the most common doubts among students is "Why did I need to use dynamic memory allocation, I don't need it!!" or something like "Why did I need another way of creating variables?". The most common case to use dynamic memory allocation is when you don't know the input on compile time and need to manage memory at runtime of the program, what it means? For example, you have to store `n` integers from user's input and you don't know how many integers the user will store on the compile time, in this case, you have three ways...

You can create a large array of integers as in the following example we use `int numbers[500]`, the trade-off of this way is that if your user store less than 500 numbers you have a memory leak, because a smaller array would be better, and if your user wants to store more than 500 numbers your software wouldn't support it.

```c
#include <stdio.h>


int main() {
    int numbers[500];
    int counter;

    printf("How many numbers you will store? ");
    scanf("%d", &counter);

    for(int i = 0; i < counter; i++) {
        printf("Enter with number %d: ", i+1);
        scanf("%d", &numbers[i]);
    }

    /* any logic .. */

    return 0;
}
```

The other way is to use variable-length arrays (also called VLA), this solution is very close to the previous, the only difference is the use of VLA when creating the array.

```c
#include <stdio.h>


int main() {
    int counter;

    printf("How many numbers you will store? ");
    scanf("%d", &counter);

    int numbers[counter];

    for(int i = 0; i < counter; i++) {
        printf("Enter with number %d: ", i+1);
        scanf("%d", &numbers[i]);
    }

    /* any logic .. */

    return 0;
}
```

It partially solves our problem, we won't waste memory but use VLA is controversial and if you search on the internet (and I encourage you to search) you'll find a lot of people talking about why you shouldn't use VLAs. But we have two main problems with this solution, the first is that our array will be automatically allocated, it means that it will be allocated on stack memory, this is a problem because stack memory is not a large memory and if you try to allocate a large array you'll get stackoverflow error, other problem, as you know, is that automatic variables only exist on the current scope. Other small problem on this solution is that VLA was introduced on C99 and old compilers will not compile it.

The other solution is to use dynamic memory allocation with `malloc()` to reserve a block of memory with the exact size that we need, this is the best solution is almost all cases:

```c
#include <stdio.h>

#include <stdlib.h>


int main() {
    int counter;

    printf("How many numbers you will store? ");
    scanf("%d", &counter);

    int *numbers = malloc(sizeof(int) * counter);

    for(int i = 0; i < counter; i++) {
        printf("Enter with number %d: ", i+1);
        scanf("%d", &numbers[i]);
    }

    /* any logic .. */

    free(numbers);
    return 0;
}
```

This is the best solution and it's using DMA in the right way, you're using heap memory and you have full control of when you want to free this block of memory using `free()`, another good thing is that you can use `realloc()` if you want to resize our block.

#### Summarize

The necessity of dynamically allocated memory is a reality on everyday programmers day, you'll need to know how to use `malloc()` to allocate heap memory at runtime, the advantages are many, you have full control of your memory, you choose when you want to free this memory, and you gain control over memory usage of your software.

### Conclusion

Understand the difference between each type of memory allocation is good to understand correctly how your C software works under a microscope, on this post we discussed about each type and on next post we'll talk about memory layout of C programs, with both knowledge, you'll have a complete understanding of this part of C software and will write and understand your software better.

Unfortunately, this is a part of programming that I see few authors writing about, on this post and on the next I hope I can help people understand the software better.

### Final thought

If you have any question that I can help you, please ask! Send an email (otaviopvaladares@gmail.com), pm me on my [Twitter](https://twitter.com/opvaladares) or comment on this post!

### Links

**Misc**

[https://stackoverflow.com/questions/2034712/is-there-any-overhead-for-using-variable-length-arrays](What's the overhead of using VLAs)

[How to use VLAs](https://www.quora.com/How-can-I-make-an-array-with-variable-size-in-the-C-language)

[https://www.tutorialspoint.com/cprogramming/c_data_types.htm](C data types)

[http://fractallambda.com/2014/10/30/Dynamic-Static-and-Automatic-memory.html](Dynamic, static and automatic memory)

[https://www.includehelp.com/c/difference-between-automatic-auto-and-static-variables.aspx](Difference between automatic, static and dynamic variable)

[Types of variables](http://faculty.cs.niu.edu/~freedman/241/241notes/241var2.htm)


**Dynamic memory allocation**

[Wikpedia of dynamic memory allocation](https://en.wikipedia.org/wiki/C_dynamic_memory_allocation)

[What is dynamic memory allocation](https://www.geeksforgeeks.org/what-is-dynamic-memory-allocation/)

[C dynamic memory allocation](https://www.programiz.com/c-programming/c-dynamic-memory-allocation)

[Differente between static and dynamic memory allocation](https://stackoverflow.com/questions/8385322/difference-between-static-memory-allocation-and-dynamic-memory-allocation)

[https://www.tutorialspoint.com/c_standard_library/c_function_malloc.htm](C function malloc)

[https://www.tutorialspoint.com/c_standard_library/stdlib_h.htm](stdlib.h)

[http://man.he.net/?topic=malloc&section=all](Nice guide about malloc)

**Static memory allocation**

[http://mpatrol.sourceforge.net/doc/Static-memory-allocations.html](Static memory allocation)

[https://www.geeksforgeeks.org/static-variables-in-c/](Static variables in C)

[https://en.wikipedia.org/wiki/Static_variable](Wikipedia page of Static variable)

[https://stackoverflow.com/questions/959889/difference-between-global-and-static-global](Difference between global and static global)

[https://stackoverflow.com/questions/44954524/difference-between-global-and-static-variable-in-c](Difference between the global and static variable in C)

[https://stackoverflow.com/questions/2271902/static-vs-global](Static vs Global variables)

[http://codingstreet.com/c-basic-questions/what-is-difference-between-global-and-static-variables-in-c/](Difference between the global and static variable in C)

[Local global and static variable](https://overiq.com/c-programming-101/local-global-and-static-variables-in-c/)
