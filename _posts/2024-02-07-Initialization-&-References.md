---
title: "Initialization & \rReferences"
author: Sillycheese
date: 2024-02-06 00:00:00 +0800
categories:
  - 语言相关
  - 关于真正的C++
tags: []
---
## Uniform initialization
curly bracket initialization. Available for all types, immediate initialization on declaration!
可以使用等号，也可以不使用
以下是例子：
```
std::vector vec{1,3,5}; 
std::pair numSuffix1{1,"st"};
Student s{"Sarah" , "CA" , 21};
// less common/nice for primitive types, but possible! 
int x{5}; string f{"Sarah"};
```
## Auto
Keyword used in  lieu of type when  declaring a variable, tells  the compiler to deduce  the type.
简单来说便是用来定义变量的，变量的类型会被编译器推导。
> Don’t overuse auto
{: .prompt-warning }

> Can’t deduce the type b/c no value provided
{:.prompt-warning }
## Structured Binding  
Structured binding lets you initialize directly from the contents of a struct.
### Before
```
auto p =  
std::make_pair(“s”, 5);  
string a = p.first;  
int b = p.second;
```
### After
```
auto p =  
std::make_pair(“s”, 5);  
auto [a, b] = p;  
// a is string, b is int  
// auto [a, b] =  
std::make_pair(...);
```

>This works for regular structs, too. Also, no nested structured binding.
{: .prompt-tip }
## Reference
Reference: An alias (another name) for a named variable
```
void changeX(int &x){
x=0;
}
void keepX(int x){
x = 0;
}  
int a = 100;
int b = 100;  
changeX(a); // a becomes a reference to x
keepX(b); // b becomes a copy of x  
cout << a << endl; //0
cout << b << endl; //100
```
## Standard C++ vector
### References to variables
```
vector<int> original{1, 2};  
vector<int> copy = original;  
vector<int>& ref = original;
original.push_back(3);  
copy.push_back(4);  
ref.push_back(5);
cout << original << endl; // {1, 2, 3, 5}  
cout << copy << endl; // {1, 2, 4}  
cout << ref << endl; // {1, 2, 3, 5}  
```

> `=`automatically makes a copy! Must use & to avoid this.
{: .prompt-tip }
## Const and Const References
`const` indicates a variable can’t be modified!
## Recap

> Remember: C++, by default, makes copies when we do variable assignment! We
need to use & if we need references instead. 
{: .prompt-info }
### When do we use references/const references?
- If we’re working with a variable that takes up little space in memory (e.g. int, double), we don’t need to use a reference and can just copy the variable
- If we need to alias the variable to modify it, we can  use references
- If we don’t need to modify the variable, but it’s a big  variable (e.g. std::vector), we can use const references