---
title: Template Classes
author: Sillycheese
date: 2024-02-21 12:00:00 +0800
categories:
  - 语言相关
  - 关于真正的C++
tags:
  - cpp
---
## Template Classes

语法的例子：
```
//mypair.h
template<typename First, typename Second> class MyPair {  
public:  
First getFirst();  
Second getSecond();

void setFirst(First f);  
void setSecond(Second f);
private:  
First first;  
Second second;  
};

//mypair.cpp
#include “mypair.h”

template<class First, typename Second>  
First MyPair<First, Second>::getFirst(){  
return first;  
}
```

### Member Types
例如，如果你有一个模板类或者函数，需要一个指向某种类型的指针作为迭代器，你可以使用这样的语法来定义一个通用的迭代器别名。这样，你可以在不同的上下文中使用`iterator`，而不用关心具体的指针类型
```
//vector.h  
template<typename T> class vector {  
public:  
using iterator = T* // something internal like T*  
iterator begin();  
}

//vector.cpp  
template <typename T>  
typename vector<T>::iterator vector<T>::begin() {...}
```

## Const Correctness
让参数成为常量意味着在函数内部，你不能修改这些参数的值

### const-interface
在C++中，`const` 关键字可以用于成员函数的声明，以指示该成员函数不会修改对象的状态。这样的成员函数被称为常量成员函数。当一个成员函数被声明为 `const` 后，它表示该函数对对象的状态没有修改的权限。
下面是例子
```
class MyClass {
public:
    void regularFunction() {
        // 这个函数可以修改对象的状态
    }

    void constFunction() const {
        // 这个函数不能修改对象的状态
    }
};
```

### static_cast and const_cast
- static_cast<new-type>( expression );  
- Used to convert from one type to another  
- Example: int my_int = static_cast<int>(3.1);  
- CANNOT BE USED WHEN conversion would cast away constness

- const_cast<new-type>( expression );
- const_cast can be used to cast away (remove) constness  
- Allows you to make non-const pointer or reference to const-object

###  **区别总结：**

- `static_cast` 主要用于合法类型之间的转换，不处理 `const` 属性。
- `const_cast` 主要用于处理 `const` 属性，可以用于添加或移除 `const`。
- 两者都需要谨慎使用，因为它们可以导致类型不安全或未定义的行为。




