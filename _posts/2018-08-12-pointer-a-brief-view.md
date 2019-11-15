---
layout:       post
title:        "Pointers, a brief view"
subtitle:     "Understanding the basics"
date:         2018-08-12 13:10:00
author:       "Otavio H. P. Valadares"
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

# A brief view of Pointer

Some weeks ago I was reading [Pat Shaughnessy, Ruby Under a Microscope](https://www.amazon.com/Ruby-Under-Microscope-Illustrated-Internals/dp/1593275277/ref=sr_1_1?s=books&ie=UTF8&qid=1534089218&sr=1-1&keywords=Pat+Shaughnessy) that is an excellent book about all the engineering behinds Ruby language, and when reading, sometimes I was feeling lost with myself when reading some C codes from Ruby implementation then I decided to stop reading the book and start a little review about C programming language because the last time that I wrote some C code that wasn't on Arduino was ~3 years ago at my university.
When I was at my first university, I clearly remember how some students feel lost and some students feel delighted with the theory and concept of pointers, and for me, was fascinating.

On this post, I'll try to explain in a simple and practical way all the theory behind C pointers, for on the next posts I write about topics like Static and Dynamic memory allocation and function pointers.

It's easy to create programs impossible to read and alter if you don't give the appropriate attention to the pointers that you're creating.

## This post talk about

- What are pointers
- How to use pointers
- Strings and Arrays
- Pass by reference and pass by value

## The computer memory

To understand pointers correctly, first, we need to understand from a top-level view of how the computer memory works.

The computer memory is like an array, is a block of consecutively memory cells order by their addresses, that memory can be manipulated alone or in groups.

![Computer Memory](https://s3.amazonaws.com/garagelabio/pointers/memory.png)

When you create an integer for example:

```c
int counter = 0;
```

The C Lang picks 4 consecutive memory cells (bytes) from the memory and stores the value that you assigned to the variable.

![Memory with integer allocated](https://s3.amazonaws.com/garagelabio/pointers/memory_allocated+(1).png)

And remember each memory cells that you requested to store your integer have a memory address that can be referenced by an hexadecimal.

![Address of allocated memory](https://s3.amazonaws.com/garagelabio/pointers/memory_allocated_address+(1).png)

*This address will vary on each execution of the program.*

To see this address of a variable in C we can use the ampersand operator:

```c
int counter = 0;

printf("Variable Address: %p\n", &counter);
```

```
Variable Address: 0x7ffeccd5f0d4
```

*The %p is the correct specifier to output memory address*

Another good experiment to do is to create two variables and see the address of each one:

```c
int counter = 0;
int age = 0;


printf("Variable Address: %p\n", &counter);
printf("Variable Address: %p\n", &age);
```

```
Variable Address: 0x7ffe3606fd60
Variable Address: 0x7ffe3606fd64
```

Pay attention to how C allocates the variable age exactly 4 bytes after the first one, this is because the integer needs 4 bytes to be stored.

The memory address is very interesting and would be good that we can store it at variables and make nice operations this, and that's is what pointers consist.

## Pointers

Pointers in computer science is an object (usually a variable) that stores the memory address of another value. In C you can declare a pointer using an asterisk before the name of the variable and store an address of a variable of the same type of the pointer.

```c
int counter = 0;

int *pointerCounter = &counter;
```

*Note: Pointers always point to a specific data type, we have one exception on type void.*

In C we can see the value stored at the address of pointed memory too, for this, use the asterisk before the name of the pointer:

```c
printf("Pointer value of the pointed memory: %i\n", *pointerCounter);
```

*When a pointer is pointing to a value, the pointer only stores the address of the first byte of this value, when asked, C uses the data type of the pointer to walk over the next `n` bytes to see the full pointed value.*

*For example, if we're pointing to an integer and the first byte of this integer is locatted on `0x7ffe3bc5e164` (see the previus image), the pointer will only store the `0x7ffe3bc5e164` and when asked for the vallue will know to walk +4 right because integer is a datatype of 4 bytes.*

To understand better, is nice to examine and run the fallowing example:

```c
int counter = 10;
int *pointerCounter = &counter;

printf("Variable counter address: %p\n", &counter);

printf("Pointer value: %p\n", pointerCounter);

printf("Pointer address: %p\n", &pointerCounter);

printf("Pointer vale of the pointed memory: %i\n", *pointerCounter);
```

```
Variable counter address: 0x7ffe19cd26fc
Pointer value: 0x7ffe19cd26fc
Pointer address: 0x7ffe19cd2700
Pointer vale of the pointed memory: 10
```

*Is nice to note that address of counter variable is exactly the value pointed by pointer*

You can do arithmetic and logic operations with pointers too, on this example I sum 10 to the value pointed by `pointerCounter`.

```c
int counter = 10;
int *pointerCounter = &counter;

*pointerCounter = *pointerCounter + 10;

printf("Pointer vale of the pointed memory: %i\n", *pointerCounter);

printf("Pointer vale of the pointed memory: %i\n", counter);
```

```
Pointer vale of the pointed memory: 20
Pointer vale of the pointed memory: 20
```

*"Ok, this is so cool but what can I do with that? I'm only storing the address of some value into one kind of variable."*

With pointers we can do a lot of things, one of the main things is pass values by reference to functions.

## Pass by reference and pass by value

When you pass a something to a function in C and make changes to it, that changes won't exist outside of that function scope, what its means?

```c
void sum(int counter, int age) {
    counter = counter + age;

    printf("counter value: %i\n", counter);
}

int main() {
    int counter = 10;
    int age = 5;

    sum(counter, age);

    printf("response value: %i\n", counter);
}
```

```c
counter value: 15
response value: 10
```

On this example, we send the `counter` variable to the void function `sum` that stores at `counter` the sum of `counter` and `age`, after this we print the value of `counter` inside the function an we see the value 15 printed, but on the printf outside the function at main, we got 10, why? This is what we call pass by value and C passes arguments to function by value, almost all languages pass by value (Java, C#, Ruby, Python), and there isn't direct way for the called function to alter a variable in the calling function.

![Pass by value](https://s3.amazonaws.com/garagelabio/pointers/pass_by_value.png)

But in C we can do what we call pass by reference, we call the function passing variable address and receiving as a pointer, doing this you're saying to function "Hey, that's the value memory address, pick this and alter as you want".

![Pass by reference](https://s3.amazonaws.com/garagelabio/pointers/pass_by_reference.png)

```c
void sum(int *counter, int age) {
    *counter = *counter + age;

    printf("counter value: %i\n", *counter);
}

int main() {
    int counter = 10;
    int age = 5;

    sum(&counter, age);

    printf("response value: %i\n", counter);
}
```

If you run this code you'll realize that on last printf the counter is 15, that's why we've passed the counter address to sum function, and at function we do the operation directly on counter address on memory. This is what we call pass by reference, we aren't passing the value to function but it's reference at memory.

## What's next?

On this post we talked about all the basics behind pointers in C, it's not a simple topic and if you don't understood keep trying and run the examples of this post on your own machine, you can also read the sections 5.1 and 5.2 of the legendary [K&R C Programming Language](https://www.amazon.com/Programming-Language-2nd-Brian-Kernighan/dp/0131103628) that's on my opinion is the best book of ANSI C.

On next three posts I'll talk more about pointers, I will write about pointers in arrays and strings, dynamic and static memory allocation and function pointers.

Feel free to ask any questions at my [twitter](https://twitter.com/valadaresotavio), email or comment this post.
