---
title: 二分查找树及其相关操作
author: Sillycheese
date: 2023-11-19 21:00:00 +0800
categories: [数据结构和算法相关,关于二分查找树]
tags: [Binary Search，tree，c++]

---

a最近学到了查找技术这里，在学完折半查找情况下，也顺便学了下二分查找树。当然未来也肯定会学习B树的。

这里便以一道上机题目（是本学期最后的实验题哦，~~嘿嘿，第一个作对！~~）

还是有点坑点的，一些问题我会在相关代码处注释。

## Description

 随着互联网技术的飞速发展，如何从海量数据中查找所需内容，不仅是科研人员关注的热点问题，许多IT公司也先后推出了各自的搜索引擎，如：Google、百度、Bing等。搜索引擎的核心是如何对Web网页构建有效的索引，以便能够快速查找和匹配查询关键词，并及时地将搜索结果返回给用户。

在这个实验中，请实现一个英文单词的二叉查找树，并可根据输入的英文单词进行搜索，同时可给出单词出现的次数。具体要求如下：

（1）构造二叉查找树：
①读入文本内容，过滤掉阿拉伯数字和标点符号，并将英文字母的大写形式全部转换成小写形式。
②按照英文字母表的顺序构造英文单词的二叉查找树。当两个英文单词的首字母相同时，按第二个字母进行排序，依次类推。
③当待插入的单词已在二叉查找树中，则将该单词的出现次数增1。

（2）遍历二叉查找树：
①搜索：输入一个待检索单词，在二叉查找树中进行查找，如果能找到该单词，则输出该单词及其出现次数；
②实现二叉查找树的中序遍历，并将遍历结果输出到屏幕上。

（3）删除结点：给定一个停用词列表（停用词是指对搜索没有作用的词，如：of, and, a, an, the等等），将二叉查找树中的属于停用词表中的单词依次删除（不仅删除结点，还需清空记录该单词位置信息的单链表）。

## Input

输入有以下四种情况：

当输入大写英文字母'T'时，表示下一行是文本内容，包含若干英文单词、标点符号以及阿拉伯数字，用于构建二叉查找树。文本内容以字符‘@’结束，文本中的每个英文单词的长度不超过30个字母。

当输入大写英文字母'S'时，表示后面跟着的若干行都是停用词，每个停用词占一行，当某行是字符‘#’时，表示停用词输入完毕。对每个停用词，都需要删除二叉查找树中的相应结点，即：每输入一个停用词，执行一次删除结点的操作。

当输入大写英文字母'V'时，表示中序遍历二叉查找树。遍历结果中的每个单词占一行，先输出该单词，然后输出一个空格，再输出该单词出现的次数。

当输入大写英文字母'Q'时，表示后面跟着的若干行都是查询词，每个查询词占一行，当某行是字符‘#’时，表示查询词输入完毕。对每个查询词，都需要在二叉查找树中的搜索相应结点，如果找到，则输出该单词及其出现次数，如果未找到，则输出-1。每个查询词的查询结果占一行，先输出该单词，然后输出一个空格，再输出该单词出现的次数。

当输入大写英文字母'X'时，表示输入结束。

## Output

按照输入的要求输出相应结果。提示：只有在'V'或者'Q'状态下，才有内容需要输出。Sample Input

```
T
According to characteristics of Mongolian word formation, a method for removing inflectional suffixes from word images of the Mongolian Kanjur is proposed in this paper. @
V
Q
the
#
S
after
against
all
almost
also
although
among
an
and
#
Q
the
#
S
the
their
then
there
therefore
these
they
this
those
though
three
to
two
#
Q
the
#
V
T
This paper presents a new method to recognize machine printed traditional Mongolian characters by using back propagation (BP) neural networks. @
Q
the
#
V
Q
mongolian
ocr
#
X
```

## Sample Output

```
a 1
according 1
characteristics 1
for 1
formation 1
from 1
images 1
in 1
inflectional 1
is 1
kanjur 1
method 1
mongolian 2
of 2
paper 1
proposed 1
removing 1
suffixes 1
the 1
this 1
to 1
word 2
the 1
the 1
-1
a 1
according 1
characteristics 1
for 1
formation 1
from 1
images 1
in 1
inflectional 1
is 1
kanjur 1
method 1
mongolian 2
of 2
paper 1
proposed 1
removing 1
suffixes 1
word 2
-1
a 2
according 1
back 1
bp 1
by 1
characteristics 1
characters 1
for 1
formation 1
from 1
images 1
in 1
inflectional 1
is 1
kanjur 1
machine 1
method 2
mongolian 3
networks 1
neural 1
new 1
of 2
paper 2
presents 1
printed 1
propagation 1
proposed 1
recognize 1
removing 1
suffixes 1
this 1
to 1
traditional 1
using 1
word 2
mongolian 3
-1
```



以下则为代码演示

### Code

```c++
#include<iostream>
#include<cstring>
using namespace std;
class Node {
private:
 
    string key; //英文单词
    int count; //出现次数
    Node* leftChild;
    Node* rightChild;
public:
    Node(string k) {
        key = k, count = 1;leftChild = NULL;rightChild = NULL;
    }
    friend class  Binary;
};
class Binary {
 
private:
 
    Node* root;
 
public:
 
    Binary() {
        root = NULL;
    }
    
 //查找temp
    Node* find(string temp){
        Node* current = root;
        while (current != NULL) {
            if (current->key == temp)
            {
                return current;
            }
            else if (temp > current->key) {
                current = current->rightChild;
            }
            else {
                current = current->leftChild;
            }
        }
        return NULL;
    }
 
 //中序遍历并输出
    void print(Node*node) {
        if (node!=NULL) {
            print(node->leftChild);
            cout << node->key << " " << node->count << endl;
            print(node->rightChild);
        }
    }
 
    //插入新节点前的过滤工作，过滤掉数字与符号，大写字母转化为小写字母
    string insertHelp(string temp) {
        string new_temp = {};
            for (int i = 0;i < temp.length();i++) {
                if (temp[i] >= 65 && temp[i] <= 90) {
                    temp[i] += 32;
                    new_temp += temp[i];
                }
                else if (temp[i] >= 97 && temp[i] <= 122)
                {
                    new_temp += temp[i];
                }
                else {
                    new_temp = new_temp;
                }
            }
         
        return new_temp;
    }
 
 //插入新节点
    void insert(string temp) {
        temp = insertHelp(temp);
        //这里通过排除掉空余项，防止过滤出的空余项仍累计count，导致后续操作出现问题
        if (temp != "") {
            if (find(temp) != NULL) {
                Node* current = find(temp);
                current->count++;
            }//已有的结点直接加count
            else {
                if (this->root == NULL)
                {
                    this->root = new Node(temp);
                }//root为null时先建立root
                else {
                    Node* current = root;
                    Node* parent;
                    while (true) {
                        parent = current;
                        if (temp < current->key) {
                            current = current->leftChild;
                            if (current == NULL) {
                                parent->leftChild = new Node(temp);
                                break;
                            }//遍历左子树，在合适位置插入
                        }
                        else {
                            if (temp > current->key) {
                                current = current->rightChild;
                                if (current == NULL) {
                                    parent->rightChild = new Node(temp);
                                    break;
                                }
                            }//与上面同理
                        }
                    }
 
                }
            }
        }
    }
    //删除工作（三种情况）
    void del(string temp) {
        Node* node = root;
        root= deletel(temp,node);
    }
    //用于寻找最小的节点（用于删除的一种情况）
    Node* findSmallest(Node* node) {
        if (node->leftChild == NULL) {
            return node;
        }
        else {
            return findSmallest(node->leftChild);
        }
    }
 //递归删除的详细过程
    Node* deletel(string temp,Node* node) {
        if (node != NULL) {
            if (temp < node->key) {
                node->leftChild = deletel(temp, node->leftChild);
            }
            else if (temp > node->key) {
                node->rightChild = deletel(temp, node->rightChild);
            }
            else {
                if (node->rightChild == NULL && node->leftChild == NULL){
                    return NULL;
 
                }
                else if (node->leftChild == NULL) {
                    return node->rightChild;
                }
                else if (node->rightChild == NULL) {
                    return node->leftChild;
                }
                else {
                    Node* min_node = findSmallest(node->rightChild);//替换成最小的右子树
                    node->key = min_node->key;
                    node->count = min_node->count;
                    node->rightChild = deletel(min_node->key, node->rightChild);//删除掉最小的右子树
                }
            }
             
        }
        return node;
    }
 //输出查找出的节点详细信息
    void printFind(string temp) {
        Node* node = find(temp);
        if (node == NULL) {
            cout << -1 << endl;
        }
        else {
            cout << node->key << " " << node->count << endl;
        }
}
 
 //获得根节点
    Node* getRoot() {
        return root;
    }
 
 
 
};
int main() {
    char option;
    Binary* p = new Binary();
    while(cin>>option){
        if (option == 'X')
        {
            return 0;
        }
        else if(option== 'T') {
            string temp;
            while (cin>>temp)
            {
                if(temp!="@")
                p->insert(temp);
                else {
                    break;
                }
            }
             
        }
        else if (option == 'S') {
            string temp;
            while (cin >> temp) {
                if (temp != "#")
                    p->del(temp);
                else {
                    break;
                }
            }
        }
        else if (option == 'V') {
            Node* node = p->getRoot();
            p->print(node);
 
        }
        else if (option == 'Q') {
            string temp;
            while (cin>>temp) {
                if(temp != "#")
                p->printFind(temp);
                else {
                    break;
                }
            }
        }
        }
     
}
```

## Final

最后，这个学期的所有实验均已完成（~~虽然才六个......~~)

接下来的学习计划大概仍会围绕数据结构与算法、以及一些很有意思的数学知识构成。

(╹ڡ╹ )学点有意思的才是学习的真正真谛嘛，在非紧急情况也不能太功利哦(●ˇ∀ˇ●)