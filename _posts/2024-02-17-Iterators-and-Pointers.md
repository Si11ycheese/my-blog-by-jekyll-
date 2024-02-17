---
title: Iterators and Pointers
author: Sillycheese
date: 2024-02-17 12:00:00 +0800
categories:
  - 语言相关
  - 关于真正的C++
tags:
  - cpp
---
## **All containers are collections of objects**
All containers implement iterators, but they’re not all the same!

- All iterators implement a few shared operations:
	- Initializing -> iter = s.begin();
	- Incrementing -> ++iter;
	- Dereferencing -> * iter;
	- Comparing -> iter != s.end();
	- Copying -> new_iter = iter;

## Structure binding
Structured binding 是 C++17 引入的一个特性，它允许将一个结构化的数据类型（如 std::tuple 或 std::pair）分解为单独的变量。这样，可以更方便地访问和使用这些变量，而不必显式地使用 std::get 或其他方式。
例子：
```
std::map<int, int> map{{1, 6}, {2, 8}, {0, 3}, {3,9}};  
for(auto iter = map.begin(); iter != map.end(); iter++) {  
const auto& [key, value] = *iter; // structured binding!  
}
```
这就是c++的for-each loop！
## Pointer
Iterators are a particular type of pointer!  
Iterators “point” at particular elements in a container.
Pointers can “point” at any objects in your code!
### Memory and You
Variables created in your code take up space on your  
computer.
They live in memory at specific addresses.
Pointers reference those memory addresses and not the  
object themselves!
### Dereferencing
Pointers are marked by the asterisk (*) next to the type of  
the object they’re pointing at when they’re declared.
The address of a variable can be accessed by using & before  
its name, same as when passing by reference!
If you want to access the data stored at a pointer’s address,  
dereference it using an asterisk again.

## What’s the difference?
● Iterators are a type of pointer!  
● Iterators have to point to elements in a container, but  
pointers can point to any object!  
- Why is this? All objects stored inside the big  
container known as memory!  
● Can access memory addresses with & and the data at  
an address/pointer using *  
