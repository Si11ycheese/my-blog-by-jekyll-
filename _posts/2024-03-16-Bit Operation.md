---
title: Bit Operations
author: Sillycheese
date: 2024-03-16 12:00:00 +0800
categories:
  - 关于C
  - 关于体系结构
tags:
  - C
---
记录一下遇到的三个位操作的训练，学习大佬的智慧！

## Problems
You may **ONLY** use bitwise operations such as and (&), or (|), xor (^), not (~), left shifts («), and right shifts (»). You may not use any for/while loops or conditional statements. You also may not use modulo (%), division, addition subtraction, or multiplication for this question.
```
// Return the nth bit of x.
// Assume 0 <= n <= 31
unsigned get_bit(unsigned x, unsigned n);

// Set the nth bit of the value of x to v.
// Assume 0 <= n <= 31, and v is 0 or 1
void set_bit(unsigned *x, unsigned n, unsigned v);

// Flip the nth bit of the value of x.
// Assume 0 <= n <= 31
void flip_bit(unsigned *x, unsigned n);
```

## Solutions

```
#include <stdio.h>  
#include "bit_ops.h"  
  
// Return the nth bit of x.  
// Assume 0 <= n <= 31  
unsigned get_bit(unsigned x,  
                 unsigned n) {  
    // YOUR CODE HERE  
    // Returning -1 is a placeholder (it makes    // no sense, because get_bit only returns    // 0 or 1)  
    unsigned tempx=x;  
    tempx=x>>n;  
    if((tempx&1)==1){  
        return 1;  
    }else{  
        return 0;  
    }  
   // return -1;  
}  
// Set the nth bit of the value of x to v.  
// Assume 0 <= n <= 31, and v is 0 or 1  
void set_bit(unsigned * x,  
             unsigned n,  
             unsigned v) {  
    // YOUR CODE HERE  
    unsigned temp=~(1<<n);  
    *x=(*x&temp);  
    *x=(*x)|(v<<n);  
}  
// Flip the nth bit of the value of x.  
// Assume 0 <= n <= 31  
void flip_bit(unsigned * x,  
              unsigned n) {  
    // YOUR CODE HERE  
    (*x)^=(1<<n);  
}
```

## Recap

-  `&` : 按位与运算，如果是1则不影响原来的值。
- `|`  :按位或运算，如果是0则不影响原来的值
- `^`  :按位异或运算，如果是0则不影响原来的值