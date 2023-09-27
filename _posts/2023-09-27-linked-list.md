---
title: 关于链表逆序的递归与非递归方法
author: Sillycheese
date: 2023-09-27 22:20:00 +0800
categories: [数据结构和算法相关,关于链表]
tags: [linked list]
---

研究了一下61B课程中的三种链表，发现甚是好看。简直是面向对象中最标准与最美丽的写法了。相比之下，我之前写的链表或者其他数据结构相关是如此的丑陋不堪，不堪入目/(ㄒoㄒ)/~~

 困扰我许久的问题便是链表的逆序问题。

**可我会做的也只是在初始化链表的时候做点手脚，使得输入的链表输出时变成逆序的**



但是如果要对一个现成的链表做手脚该怎么做呢？

(●ˇ∀ˇ●)这里提供两个办法——分别是递归和非递归的办法。

上代码！



## Code is here~~~

```c++
#include<iostream>
using namespace std;
struct Node {
	int data;
	Node* next;
};
Node* head = new Node; //头节点

//初始化链表，使其呈现为0->1->2的结构
void New() {
	head = new Node;
	head->data = 0;
	head->next = new Node;
	head->next->next = new Node;
	head->next->data = 1;
	head->next->next->data = 2;
	head->next->next->next = NULL;
	}

//非递归，通过不断的将一个新的头节点一步一步向原链表的后方运输与搭建新链表，由此实现链表的逆序
Node * Reverse(Node *head) {
	Node* temp =head ;Node *Reverse_head = NULL;
	Node* temp_t = NULL;
	while (temp != NULL) {
		temp_t = temp->next;
		temp->next = Reverse_head; //这一部分有点像尾插法，只不过动的是新创建的头指针
		Reverse_head = temp;
		temp = temp_t;
	}
	return Reverse_head;


}

//递归，通过不断递归使得最终的Base Case为头节点指向最后的节点;而每次的递归需要做的只是令新的头节点的前一位与后一位相连，并删掉该节点
Node* Reverse_recursive(Node* head) {
	if (head==NULL || head->next == NULL) {
		return head;
	}
	else {
		Node*temp=Reverse_recursive(head->next);
		head->next->next = head;
		head->next = NULL;
		return temp;
	}

}
//输出链表
void PrintList(Node* head) {
	Node* temp = head;
	while (temp != NULL) {
		cout << temp->data << endl;
		temp = temp->next;
	}
}
```





## Final

总之思路就是想办法创建一个新的头节点，通过老节点将其一步一步送到最后的节点。

加深了对于链表的理解与思考

~~果然这还是太高深莫测了，稍微理解偏差一点就会懵~~



