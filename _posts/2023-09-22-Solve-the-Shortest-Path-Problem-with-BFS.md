---
title: 用BFS解决迷宫最短路径问题的一些思考
author: Sillycheese
date: 2023-09-22 00:20:00 +0800
categories: [数据结构和算法相关,关于DFS与BFS]
tags: [BFS,C++,Algorithm,queue,面向对象] 
---

## 废话环节

作为一个刚刚步入大二的没几周的大学生，从第二周就被要求去搞定最短路径，解决DFS和BFS算法是吧。

​    ~~魏XX你坏事做尽！！！~~

**(╹ڡ╹ )**但是还是有很多益处的，接下来我会来写写关于这两个算法的内容。

~~因为我很懒(●ˇ∀ˇ●)，所以暂时只更新BFS的~~



## 题目浏览



以一个M×N的长方阵表示迷宫。设计一个程序，对任意设定的迷宫，求出一条从入口到出口的最佳通路。**
**

实验目的：继续熟练掌握栈的特点；灵活应用栈和队列。

具体要求：

（1）以一个M×N的长方阵表示迷宫，1和0分别表示迷宫中的通路和障碍。设计一个程序，对任意设定的迷宫，求出一条从入口到出口的最佳通路，或得出没有通路的结论。（所谓最佳通路是指在所有的通路中输出步长最短的一条通路。）

（2）首先创建一个迷宫，输入格式为：M N，指定迷宫的行数和列数，然后按行输入迷宫的每一行的布局信息（参见输入示例）。求得的通路以三元组(i, j, d)的形式输出（每行输出5组），其中：(i, j)表示迷宫的坐标，d表示走到下一坐标的方向（值为1、2、3、4，分别对应右、下、左、上方向）。迷宫入口坐标(左上角)为（1,1），出口坐标为右下角（M,N）,若有通路，则最后输出的坐标三元组格式为（M,N,0）。

（3） 本题目要求可以连续输入多组迷宫数据进行测试，若输入迷宫的行数和列数分别为0，则输入结束。注意输出格式的要求。

（4）提示：用栈和队列都可实现。使用栈从所有可能的通路中寻找最短路径。使用队列可通过广度优先算法直接确定最短路径。

### Input

**示例输入：**

4 5      ----------迷宫的行数和列数

1 0 0 0 0

1 1 1 0 1

1 0 0 1 1

1 1 1 1 1

4 4      ----------迷宫的行数和列数

1 0 0 1

1 0 1 1

1 0 0 1

1 0 1 1

0 0      ----------输入结束标志

### Output

若迷宫有通路，则输出迷宫的通路，以每行输出5组的形式控制输出格式；若迷宫没有通路则输出“没有通路”。如上例的输入对应的输出为：

(1,1,2)(2,1,2)(3,1,2)(4,1,1)(4,2,1)
(4,3,1)(4,4,1)(4,5,0)

没有通路

## 对于该题目的思考过程

是的，第一眼看到这个题目完全懵逼。

 ~~我是在做竞赛题吗？~~

然后通过强大的搜索引擎了解到BFS与DFS两个算法，并首先去运用DFS去尝试解决该问题

~~这不是讲BFS的吗？~~

但是失败了，也许DFS是最快找到路径的，但是找到的路径未必是最短的。而我便因为这个特性而忽略了**最短**这个特点。

以下是DFS初期实现

```c++
#include <iostream>
#include <vector>
#include <stack>
using namespace std;

struct Point {
    int x;
    int y;
    int direction;
    Point(int _x, int _y, int _direction) : x(_x), y(_y), direction(_direction) {}
};

// 检查坐标是否合法
bool isValid(int x, int y, int m, int n) {
    return x >= 0 && x < m && y >= 0 && y < n;
}

// 检查当前坐标是否为出口
bool isExit(int x, int y, int m, int n) {
    return x == m - 1 && y == n - 1;
}

// 输出路径
void printPath(stack<Point>& path, int m, int n) {
    vector<Point> temp;
    while (!path.empty()) {
        temp.push_back(path.top());
        path.pop();
    }
    int count = 0;
    for (int i = temp.size() - 1; i >= 0; i--) {
        cout << "(" << temp[i].x + 1 << "," << temp[i].y + 1 << "," << temp[i].direction << ")";
        count++;
        if (count == 5) {
            cout << endl;
            count = 0;
        }
        if (i == 0)
        {
            cout << "(" << m << "," << n << "," << 0 << ")" << endl;
        }

    }

}
// 求解迷宫最佳通路
void findPath(vector<vector<int>>& maze, int m, int n) {
    stack<Point> path;
    int visited[100][100];
    for (int i = 0;i < 100;i++)
    {
        for (int j = 0;j < 100;j++) {
            visited[i][j] = 0;
        }
    }
    // 入口坐标
    int startX = 0;
    int startY = 0;

    // 出口坐标
    int endX = m - 1;
    int endY = n - 1;

    // 右、下、左、上四个方向
    int dx[] = { 0, 1, 0, -1 };
    int dy[] = { 1, 0, -1, 0 };

    // 将起点入栈并标记为已访问
    path.push(Point(startX, startY, 0));
    visited[startX][startY] = 1;

    while (!path.empty()) {
        Point curr = path.top();
        path.pop();

        int x = curr.x;
        int y = curr.y;
        int direction = curr.direction;
  
        if (isExit(x, y, m, n)) {
            // 输出路径
            printPath(path, m, n);
            return;
        }

        for (int i = direction; i < 4; i++) {
            int newX = x + dx[i];
            int newY = y + dy[i];

            if (isValid(newX, newY, m, n) && maze[newX][newY] == 1 && visited[newX][newY]==NULL) {
                path.push(Point(x, y, i + 1));
                visited[newX][newY] = 1;
                path.push(Point(newX, newY, 0));
                break;
            }
        }
    }

    // 没有通路
    cout << "没有通路" << endl;
}

int main() {
    int m, n;
    int count = 0;
    while (cin >> m >> n) {
        if (m == 0 && n == 0) {
            break;
        }
         count++;
        vector<vector<int>> maze(m, vector<int>(n));

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                cin >> maze[i][j];
            }
        }

        findPath(maze, m, n);
    }
}
```

最后的结果就是在oj的答案错误里度过了每一天（恼

**所以为什么不尝试BFS呢（喜**

## BFS代码展示

不多说了，直接发代码吧

### **普通版**

```c++
#include <iostream>
#include <queue>
using namespace std;
int m, n;
int maze[100][100];
struct Point {
    int x, y, direction;
    Point(int _x = 0, int _y = 0, int _direction = 0) : x(_x), y(_y), direction(_direction) {}
};
int dy[] = { 1,0,-1,0 };
int dx[] = { 0,1,0,-1 };
void bfs()
{
    bool visited[100][100];
    for (int i = 0;i < 100;i++)
    {
        for (int j = 0;j < 100;j++) {
            visited[i][j] = false;
        }
    }
    struct Point pre[100][100];
    queue<Point> q;
    int s_x = 1, s_y = 1;
    pre[s_x][s_y].x = -1;
    pre[s_x][s_y].y = -1;
    pre[s_x][s_y].direction = 0;
    q.push(Point(1, 1, 0));
    visited[s_x][s_y] = true;
    while (q.size()) {
        Point k = q.front();
        q.pop();
        for (int i = k.direction;i < 4;i++) {
            int x = k.x + dx[i];
            int y = k.y + dy[i];
            if (x >= 1 && x <= m && y >= 1 && y <= n && maze[x][y] == 1 && !visited[x][y]) {
                visited[x][y] = true;
                q.push(Point(x, y, 0));
                pre[x][y].x = k.x;
                pre[x][y].y = k.y;
                pre[x][y].direction = i + 1;
            }
 
        }
    }
    int x = m, y = n, d = 0;
    Point p[100];
    if (pre[x][y].x == 0 && pre[x][y].y == 0)
    {
         
        cout << "没有通路" ;
        return;
    }
    else
    {
        int i = 0;
        while (x != -1 && y != -1) {
            p[i].x = x;p[i].y = y;p[i].direction = d;
            Point p = pre[x][y];
            x = p.x;
            y = p.y;
            d = p.direction;
            i++;
        }
        int count = 0;
        for (int j = i-1;j >= 0;j--)
        {
            cout << "(" << p[j].x << "," << p[j].y << "," << p[j].direction << ")";
            count++;
            if (count == 5) {
                cout << endl;
                count = 0;
            }
        }
 
    }
 
 
 
}
int main() {
 
    while (cin >> m >> n) {
        if (m == 0 && n == 0) {
            return 0;
        }
 
 
        for (int i = 1;i <= m;i++) {
            for (int j = 1;j <= n;j++) {
                cin >> maze[i][j];
            }
        }
        bfs();
        cout << endl;
    }
}
```

我觉得算法本身没有什么特别的，最重要的便是如何读取你走过路径的**结点**

而本代码便提供了一种很精妙，类似指针一样的东西，仿佛将每个节点的信息联系到了一起。

这感觉很不错！

最后再附上面向对象版吧！

### **面向对象版**

~~数组懒得初始化，自己去封装到类里(╹ڡ╹ )~~

```c++
#include <iostream>
#include <queue>
int maze[100][100];
using namespace std;
int dy[] = { 1,0,-1,0 };
int dx[] = { 0,1,0,-1 };
class Point {
    int x, y, direction;
public:
    Point(int _x = 0, int _y = 0, int _direction = 0) : x(_x), y(_y), direction(_direction) {}
    friend class Find_Path;
 
 
};
class Find_Path {
 
    int m,  n;
public:
    Find_Path(int m, int n) {
        this->m = m;
        this->n = n;
         
    }
    void bfs() {
        bool visited[100][100];
        for (int i = 0;i < 100;i++)
        {
            for (int j = 0;j < 100;j++) {
                visited[i][j] = false;
            }
        }
        struct Point pre[100][100];
        queue<Point> q;
        int s_x = 1, s_y = 1;
        pre[s_x][s_y].x = -1;
        pre[s_x][s_y].y = -1;
        pre[s_x][s_y].direction = 0;
        q.push(Point(1, 1, 0));
        visited[s_x][s_y] = true;
        while (q.size()) {
            Point k = q.front();
            q.pop();
            for (int i = k.direction;i < 4;i++) {
                int x = k.x + dx[i];
                int y = k.y + dy[i];
                if (x >= 1 && x <= m && y >= 1 && y <= n && maze[x][y] == 1 && !visited[x][y]) {
                    visited[x][y] = true;
                    q.push(Point(x, y, 0));
                    pre[x][y].x = k.x;
                    pre[x][y].y = k.y;
                    pre[x][y].direction = i + 1;
                }
 
            }
        }
        int x = m, y = n, d = 0;
        Point p[100];
        if (pre[x][y].x == 0 && pre[x][y].y == 0)
        {
 
            cout << "没有通路";
            return;
        }
        else
        {
            int i = 0;
            while (x != -1 && y != -1) {
                p[i].x = x;p[i].y = y;p[i].direction = d;
                Point p = pre[x][y];
                x = p.x;
                y = p.y;
                d = p.direction;
                i++;
            }
            int count = 0;
            for (int j = i - 1;j >= 0;j--)
            {
                cout << "(" << p[j].x << "," << p[j].y << "," << p[j].direction << ")";
                count++;
                if (count == 5) {
                    cout << endl;
                    count = 0;
                }
            }
 
        }
 
 
 
 
    }
};
 
int main() {
    int m, n;
    Find_Path *a;
    while (cin >> m >> n) {
        if (m == 0 && n == 0) {
            return 0;
        }
 
 
        for (int i = 1;i <= m;i++) {
            for (int j = 1;j <= n;j++) {
                cin >> maze[i][j];
            }
        }
        a = new Find_Path(m, n);
        a->bfs();
 
        cout << endl;
    }
}
```



## 最后

什么？还有最后？

当然是快马加鞭写出DFS在查找最短路径上的用法啦（懒

/(ㄒoㄒ)/~~要累死了

