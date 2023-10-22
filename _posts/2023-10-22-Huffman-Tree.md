---
title: Huffman以及筛选函数降低时间复杂度的方法
author: Sillycheese
date: 2023-10-22 17:00:00 +0800
categories: [数据结构和算法相关,关于Huffman]
tags: [Huffman]
---

最近学习Huffman树极其编码的相关知识，颇有收获。

毕竟有贵人相助嘛φ(゜▽゜*)♪

话不多说，步入正题。

## Basic Knowledge

针对Huffman树极其编码的基础知识，直接去看[这个](https://oi-wiki.org/ds/huffman-tree/)好了。

对于树的建立主要在于不断遍历寻找最小值以及次最小值，并通过循环不断更新与合并删除新的root

而对于编码的构成则可以通过递归来实现。

## Example

这里举例一道例题

### Description

“Huffman-树”不仅能对文本数据进行编码、译码，提高文本数据的传输效率，同时它也能对多媒体数据（如：数字图像、视频等）进行编码、译码，从而实现多媒体数据的压缩存储。目前，在Web互联网上广泛使用的JPEG图像格式就采用了Huffman编码，与其他图像格式（如：BMP、TIF等）相比，同一副图像采用JPEG格式时所需的存储空间是最少的。在这个实验中，请设计一个Huffman编/译码器，并模拟数字图像的压缩存储（编码）和解码显示（译码）的过程。

（1）构造“Huffman-树”：
①读入一个大小为N*M（N为图像的高度，M为图像的宽度）的灰度图像块，该图像中的每个像素（元素）的取值范围是0~255，取值为0表示该像素是“黑色”，取值为255表示该像素是“白色”，其他取值表示介于“黑色”和“白色”之间的灰度值。
②统计读入图像块中每种灰度值出现的次数，并去除出现次数为零的灰度值，以此作为构造“Huffman-树”所需的权值。
③说明：在构造“Huffman-树”的过程中，当有多个待合并元素的权值相同时，每次选择灰度值较小的两个元素进行合并。
（2）Huffman编码（压缩存储）：利用已建立好的“Huffman-树”，对灰度图像块进行编码，之后计算Huffman编码的压缩比并输出。压缩比的计算公式如下：**压缩比=原始图像所需比特数/压缩后图像所需比特数**。

### Input

该实验有多组测试数据。

每组测试数据的第一行包含两个以空格间隔的正整数N和M，它们的取值范围都为[1, 100]，分别表示图像的高度和宽度；之后的N行数据是一个灰度图像，每行含有M个以空格间隔的正整数P（0≤P≤255），P表示对应像素的灰度值。

当输入的N和M都为零时结束。

### Output

每组测试数据的输出占一行，输出的内容为“Huffman编码的压缩比”（保留2位小数）。

### Sample Input

```bash
8 8
162 162 162 161 162 157 163 161
162 162 162 161 162 157 163 161
162 162 162 161 162 157 163 161
162 162 162 161 162 157 163 161
162 162 162 161 162 157 163 161
164 164 158 155 161 159 159 160
160 160 163 158 160 162 159 156
159 159 155 157 158 159 156 157
0 0
```

### Sample Output

```bash
Huffman编码的压缩比为2.72:1
```

## 想法以及代码实现

先说说想法吧，构造Huffman树和Huffman编码倒是不难。这个题对于我来讲，问题首先在于这个压缩比如何求，其次便是Huffman树中筛选函数遍历问题导致的时间复杂度上升的问题如何解决。

### 先来说说压缩比

其实这个还可以，Google一下便可搜索出资料。

压缩比=原始图像所需比特数/压缩后图像所需比特数

首先我们需要知道：1byte = 8bit  一个字节是八比特。

所以**原始图像所需比特数**便是字节的数量×8.

这并不难，有意思的来了。

**压缩后的比特数如何计算呢？**

通过Huffman编码的压缩运算，我们通过字符出现的频率将其计算，并得出新的前缀编码。

前缀编码很有意思，是由'1'和'0'组成。通过这个你应该想到了什么。~~是赤裸裸的bit口牙o(*￣▽￣*)ブ~~

所以出现的频率便是该字节的数量，而新的编码的比特总数便是该字符的比特数。

所以压缩比自然也是迎刃而解了。

### 再来说说关于筛选函数

我们用来构建Huffman树以及后续计算编码所用的权值正是出自筛选函数。

就是从一堆数据中筛选每一个数据所出现的频率，并用新的数组保存。

**稍有不慎就有可能时间复杂度指数增长∑( 口 || **

所以这里给出一个O(n)的方案。

首先通过一个数组用来记录所有的数据。

再通过一个数组，用它来记录每一个数据出现的次数（用下标当作标签）。

最后创建一个新的数组，通过循环遍历来枚举值，用新数组记录下来。

这种不超过1e5的数据去重，用新数组记录下来便好。



以下是代码实现

### 代码实现

```c++
#include<iostream>
#include<iomanip>
#include<algorithm>
#include<cstring>
using namespace std;
class HNode {
    int item;
    double size;
    HNode* lchild;
    HNode* rchild;
    double code;
    friend class HuffmanTree;
};
int Set[10000] ;
HNode* temp = new HNode[10000];
int a[256]; 
class HuffmanTree {
    int N, M;
    HNode* root;
    int length;
public:
    HuffmanTree(int n, int m) {
        N = n;
        M = m;
        root = new HNode;
        length = 0;
    }
    void  createSet() {
        for (int i = 0;i < N * M;i++) {
            cin >> Set[i];
            temp[i].size = 0;
 
        }
        for (int i = 0;i < N * M;i++) {
            a[Set[i]]++;
        }
        int g = 0;
        for (int i = 0;i < 256;i++) {
            if (a[i] != 0)
            {
                while (a[i]--)
                {
                    temp[g].item = i;
                    temp[g].size++;
                     
                }
                g++;
            }
        }
        length = g;
 
    }
    //建立Huffman树
    void createHuffmanTree(HNode* forest[]) {
        root = new HNode;
        root = NULL;
        //初始化
        for (int i = 0;i < length;i++) {
            HNode* tem = new HNode;
            tem->item = temp[i].item;
            tem->size = temp[i].size;
            tem->lchild = NULL;tem->rchild = NULL;
            forest[i] = tem;
        }
        //n-1次建树
        for (int i = 1;i < length;i++) {
            int min = -1, submin;
            for (int j = 0;j < length;j++) {
                if (forest[j] != NULL && min == -1) {
                    min = j;
                    continue;
                }
                if (forest[j] != NULL) {
                    submin = j;
                    break;
                }
            }//初始化min与submin
            for (int j = submin;j < length;j++) {
                if (forest[j] != NULL) {
                    if (forest[j]->size < forest[min]->size) {
                        submin = min;
                        min = j;
                    }
                    else if (forest[j]->size < forest[submin]->size) {
                        submin = j;
                    }
                    else if (forest[j]->size == forest[min]->size && forest[j]->item < forest[min]->item) {
                        submin = min;
                        min = j;
                    }
                    else if (forest[j]->size == forest[submin]->size && forest[j]->item < forest[submin]->item) {
                        submin = j;
                    }
                }
            }//比较与筛选min与submin
            root = new HNode;
            root->size = forest[min]->size + forest[submin]->size;
            root->lchild = forest[min];
            root->rchild = forest[submin];
            forest[min] = root;      // 指向新树的指针赋给 min 位置
            forest[submin] = NULL;   // submin 位置为空
 
        }//建新树
    }
    void huffmanCodeHelper(HNode*& root, int len, int arr[], HNode*& ptr) {
        if (root != NULL) {
            if (root->lchild == NULL && root->rchild == NULL) {
                ptr->code += root->size * len;
            }
            else {
                arr[len] = 0;
                huffmanCodeHelper(root->lchild, len + 1, arr, ptr);
                arr[len] = 1;
                huffmanCodeHelper(root->rchild, len + 1, arr, ptr);
            }
        }
    }
    void huffmanCoding() {
        int len = 0;
        int arr[1000];
        HNode* ptr = root;
        ptr->code = 0;
        huffmanCodeHelper(root, len, arr, ptr);
        double old_byte = N * M * 8;
        double new_byte = ptr->code;
        double rate = old_byte / new_byte;
        cout << "Huffman编码的压缩比为" << fixed << setprecision(2) << rate << ":1" << endl;
    }
};
int main() {
    int n, m;
    while (cin >> n >> m) {
        if (n == 0 && m == 0) {
            return 0;
        }
        HuffmanTree* s = new HuffmanTree(n, m);
        s->createSet();
        HNode* forest[1000];
        s->createHuffmanTree(forest);
        s->huffmanCoding();
        memset(temp, '\0', sizeof(temp));
        memset(a, '\0', sizeof(a));
        memset(Set, '\0', sizeof(Set));
         
    }
}
```



## Final

φ(゜▽゜*)♪

既学到了Huffman树与编码，又学到了一种去重方法。

继续学习呗，继续提问并解决。

嘿嘿~



