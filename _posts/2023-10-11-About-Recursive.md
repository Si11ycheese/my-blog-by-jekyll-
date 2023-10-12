---
title: 关于Recursive的一些感悟与心得
author: Sillycheese
date: 2023-10-11 23:00:00 +0800
categories: [特殊,About Recursive]
tags: [Recursive,Induction]
---

  最近闲暇时光一直在研究一些千奇百怪的东西。看到了很多怪东西，再结合从大一就一直让我感到好奇的递归，似乎恍惚间有了点东西。

## What is Recursive? Recursion?

 在数学意义上的**Recursive**包含了**递推**和**回归**的过程。

而在程序执行的过程中，把大的复杂问题分解成更小更简单的问题，逐级分解下去，直到问题的规模小到可以解决问题，然后再逐级向上回溯解决最初的问题。

在编程中，简单来讲，便是自己调用自己。这与迭代是一样的效果。

递归是由于f(n)扩展到f(1),再由f(1)逐渐算到f(n).

迭代则是直接向下运算，从f(1)到f(n).

## Induction and Recursion

似乎这两个有某种联系。

**Induction**拥有**Base Case**，以及函数公式本身，通过这些来进行进一步运算来验证公式本身的对与错。

让我们把其证明部分的功能砍掉，似乎就明朗许多。

以下用伪代码与文字，通过斐波那契数列来演示。

• F(0) = 0. 

• F(1) = 1. 

• For n ≥ 2, F(n) = F(n−1) +F(n−2).

而0与1则是Base Case，在递归里我的理解是防止递归本身无限调用自己，是结束运算的标志

以下是伪代码

```
function F(n)
if n=0 then return 0
if n=1 then return 1
else return F(n-1) + F(n-2)
```

因此递归完全可以运用Induction的东西来执行。只要寻找Base Case,便可以编写代码。



## some examples

比如咱们最常见的查找，既然可以用迭代写，那么递归也没问题。

首先确定Base Case，也就是能阻止无限循环的工具。

在下面的例子里明显是当index==0时存在。

确定后再回溯到Base Case，通过这一过程来完成运算。

```java
private T getREcursivehelper(IntNode p,int index){
        if(index==0){
            return p.item;
        }
        return getREcursivehelper(p.next,index-1);
   }
   public T getRecursive(int index){
        if(index>=size){
            return null;
        }
        IntNode p=sentinel;
        return getREcursivehelper(p.next,index);
   }
```

一些实际的问题也可以用递归解决，比如汉诺塔问题。

```c++
#include <iostream>
using namespace std;

void Towers(int n,char a,char b,char c){
	if(n==1){
		cout<<"Move disk "<<n<<" from"<<a<<" to "<<c<<endl;
	}
	else{
		Towers(n-1,a,c,b);
		cout<<"Move disk "<<n<<" from"<<a<<" to "<<c<<endl;
		Towers(n-1,b,a,c);
	}
}
int main(int argc, char *argv[]) {
	int n;
	cin>>n;
	Towers(n,'A','B','C');
	cout<< endl;
	
	
}
```









