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