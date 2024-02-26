---
title: Functions and Lambdas
author: Sillycheese
date: 2024-02-26 12:00:00 +0800
categories:
  - 语言相关
  - 关于真正的C++
tags:
  - cpp
---
## Lambda
```
[capture](parameters) -> return_type {
    // 函数体
}
```
## Function pointers
```
#include <iostream>

// 定义一个函数原型
int add(int a, int b) {
    return a + b;
}

int subtract(int a, int b) {
    return a - b;
}

int main() {
    // 声明一个函数指针，指向具有两个整数参数和整数返回类型的函数
    int (*operation)(int, int);

    // 将函数指针指向 add 函数
    operation = add;

    // 使用函数指针调用 add 函数
    int result_add = operation(3, 4);
    std::cout << "Result of add: " << result_add << std::endl;  // 输出 7

    // 将函数指针指向 subtract 函数
    operation = subtract;

    // 使用函数指针调用 subtract 函数
    int result_subtract = operation(7, 3);
    std::cout << "Result of subtract: " << result_subtract << std::endl;  // 输出 4

    return 0;
}

```

Everything (lambdas, functors, function pointers) can be cast to a standard  
function!

### STL FUNCTION
```
#include <iostream>
#include <functional>

// 定义一个普通函数
int add(int a, int b) {
    return a + b;
}

int main() {
    // 使用 lambda 表达式创建一个匿名函数
    auto lambda = [](int a, int b) -> int {
        return a + b;
    };

    // 使用 std::function 包装函数指针
    std::function<int(int, int)> func_pointer = add;

    // 使用 std::function 包装 lambda 表达式
    std::function<int(int, int)> func_lambda = lambda;

    // 调用包装后的函数
    int result_pointer = func_pointer(3, 4);
    int result_lambda = func_lambda(3, 4);

    std::cout << "Result of function pointer: " << result_pointer << std::endl;  // 输出 7
    std::cout << "Result of lambda function: " << result_lambda << std::endl;    // 输出 7

    return 0;
}


```

