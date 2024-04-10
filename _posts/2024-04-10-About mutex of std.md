---
title: 关于锁的一些内容与整理
author: Sillycheese
date: 2024-04-10 12:00:00 +0800
categories:
  - 关于真正的C++
tags:
  - cpp
---
## std::mutex

std::mutex：最基本的mutex类。  
    std::recursive_mutex：递归mutex类，能多次锁定而不[死锁](https://so.csdn.net/so/search?q=%E6%AD%BB%E9%94%81&spm=1001.2101.3001.7020)。  
    std::time_mutex：定时mutex类，可以锁定一定的时间。  
    std::recursive_timed_mutex：定时递归mutex类。

另外，还提供了两种锁类型：

    std::lock_guard：方便线程对互斥量上锁。

    std::unique_lock：方便线程对互斥量上锁，但提供了更好的上锁和解锁控制。

    以及相关的函数：  
    std::try_lock：尝试同时对多个互斥量上锁。  
    std::lock：可以同时对多个互斥量上锁。  
    std::call_once：如果多个线程需要同时调用某个函数，call_once可以保证多个线程对该函数只调用一次。
## std::lock_guard

std::lock_guard是一个模板类，模板类型可以是以上的四种锁，用于自动锁定解锁，直到对象作用域结束。


参考文献：https://blog.csdn.net/u012372584/article/details/96836084



## P.S.
顺便记点别的特性，~~懒得开新post了~~。

### 左值和右值

首先区分左值和右值

左值是表达式结束后依然存在的持久对象(代表一个在内存中占有确定位置的对象)

右值是表达式结束时不再存在的临时对象(不在内存中占有确定位置的表达式）

便携方法：对表达式取地址，如果能，则为左值，否则为右值

#### 小结

- 在《Effective Modern C++》中建议：**对于右值引用使用std::move，对于万能引用使用std::forward。**
- std::move()与std::forward()都仅仅做了**类型转换(可理解为static_cast转换)**而已。真正的移动操作是在移动构造函数或者移动赋值操作符中发生的
- 在类型声明当中， “&&” 要不就是一个 rvalue reference ，要不就是一个 universal reference – 一种可以解析为lvalue reference或者rvalue reference的引用。对于某个被推导的类型T，universal references 总是以 T&& 的形式出现。
- 引用折叠是 会让 universal references (其实就是一个处于引用折叠背景下的rvalue references ) 有时解析为 lvalue references 有时解析为 rvalue references 的根本机制。引用折叠只会在一些特定的可能会产生"引用的引用"场景下生效。这些场景包括模板类型推导，auto 类型推导， typedef 的形成和使用，以及decltype 表达式。

### std::move使用场景

- 在实际场景中，右值引用和std::move被广泛用于在STL和自定义类中**实现移动语义，避免拷贝，从而提升程序性能**。 在没有右值引用之前，一个简单的数组类通常实现如下，有`构造函数`、`拷贝构造函数`、`赋值运算符重载`、`析构函数`等。深拷贝/浅拷贝在此不做讲解。