---
title: Autoboxing
author: Sillycheese
date: 2024-02-02 23:00:00 +0800
categories: [语言相关,关于Java]
tags: [Java] 
---

#### Caveats of autoboxing and unboxing

- Arrays are never autoboxes or auto-unboxed, e.g. if you have an array of integers `int[] x`, and try to put its address into a variable of type `Integer[]`, the compiler will not allow your program to compile.

- Autoboxing and unboxing also has a measurable performance impact. That is, code that relies on autoboxing and unboxing will be slower than code that eschews such automatic conversions.

- Additionally, wrapper types use much more memory than primitive types. On most modern comptuers, not only must your code hold a 64 bit reference to the object, but every object also requires 64 bits of overhead used to store things like the dynamic type of the object.
  - For more on memory usage, see [this link](http://www.javamex.com/tutorials/memory/object_memory_usage.shtml) or [this link](http://blog.kiyanpro.com/2016/10/07/system_design/memory-usage-estimation-in-java/).

### Widening

For example, doubles in Java are wider than ints. If we have the function shown below:

```
public static void blahDouble(double x) {
    System.out.println(“double: “ + x);
}
```

We can call it with an int argument:

```
int x = 20;
blahDouble(x);
```

The effect is the same as if we'd done `blahDouble((double) x)`. Thanks Java!