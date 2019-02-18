---
layout:       post
title:        "Strings in C, a brief view"
subtitle:     "Understand one and for all"
date:         2018-12-23 01:10:00
author:       "Otavio H. P. Valadares"
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

Let's continue our journey through C programming language, today we'll study the strings in C which often cause confusion in the language beginners.

I would like to start this text with a famous example that usually cause confusion in programmers that came from other languages:

```c
#include <stdio.h>

char string1[3] = "hi";
char string2[3] = "hi";

int main() {
    if(string1 == string2) {
      puts("Equal!");
    } else {
      puts("Different!")
    }

    return 0;
}
```

What this program will output?

If you never programmed C or don't know how a computer language works behind scenes I think that you probably will answer "The program will print that both strings are equal, and that isn't a problem, I understand you, but the correct answer is that program will output "Different!", and on this text you'll learn why(or remember), at the end of this tutorial you'll know all details about strings ins C. The study of strings in C is important, not only to program C, but because it shows you a lot of computer science lessons and help you understand how other languages work.
