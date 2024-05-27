---
title: Sorting&Aggregation Algorithm
author: Sillycheese
date: 2024/5/26 09:51:27 +0800
categories:
  - CMU15445
tags:
  - Database
---
## Sort

关系模型/SQL 是无序的

### IN-MEMORY SORTING

如果数据 fit in 内存，我们可以用标准的搜索算法（比如快排）

否则，我们需要了解读写 disk pages 的 cost。

### TOP-N HEAP SORT

如果查询有`ORDER BY`和`LIMIT`，则DBMS只需要去扫描数据一次来寻找top-N elements.

理想情况对于堆排：如果top-N elements fit in memory

- 扫描数据一次，维持一个in-memory sorted的优先队列

### EXTERNAL MERGE SORT

`Divide-and-conquer algorithm`将数据拆分成独立的`runs`,单独将他们排序，然后将他们合并成更大的已经sorted的`runs`

- Phase #1- Sorting
  - Sort chunks of data that fit in memory and then write back the sorted chunks to a file on disk

- Phase #2- Merging
  - Combine sorted runs into larger chunks. 

#### SORTED RUN

run是k/v pairs的一个列表

k是相比较以算出排列顺序的属性

v有两种选择

- Tuple
- Record ID

### USING B+TREES FOR SORTING

