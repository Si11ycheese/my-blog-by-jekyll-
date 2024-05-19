---
title: CS188-Search with Other Agents
author: Sillycheese
date: 2024-05-12 12:00:00 +0800
categories:
  - CS188
tags:
  - AI
---

## Types of Games

### Axes

- 确定的还是随机的？
- 游戏玩家的数量是多少？
- 是否是零和游戏
- 当决定策略的时候是否了解游戏的全部信息

### Deterministic Games

- States: S(start at s0)
- Players: 一组经常轮流的玩家
- Actions：取决于玩家/状态
- Transition Function：很像常规搜索的后继函数（state and action to the next state）
- Terminal Test：游戏是否结束
- Terminal Utilities：获胜的结果（会被记录）
- Solution：也就是策略，每种可能的情况应该采取的策略

### Zero-Sum Games

简单概括来说就是争夺有限的资源。（要么赢要么输）

而 `General Games` 则有可能出现双赢情况

## Adversarial Search

### Single-Agent Trees

通俗来讲就是选择方向分支然后不断循环，树的每一个节点都可以有值来衡量状态。（也可以通过子节点的最大值来 back 到父节点，直到 root）

而 `Minimax value` 则是相对于不同 agent 最好策略的值。

不同 agent 可以轮流，从下到上计算 `Minimax value`。

感觉有点像回合制游戏。

### Game Tree Pruning

有选择性的跳过一些节点（通过一些过滤的条件），这样就是 AB Pruning

#### Alpha-Beta Pruning

```python
# a是MAX版本最好的通向root的选择，b则相反
def max-value(state,a,b):
    initialize v=-∞
    for each successor in date:
        v=max(v,value(successor,a,b))
        if v>= b:return v
    	a=max(a,v)
    return v

def min-value(state,a,b):
    initialize v=+∞
    for each successor in date:
        v=min(v,value(successor,a,b))
        if v<= a:return v
    	b=min(a,v)
    return v

```

### Evaluation Function

通常来讲，这个函数越好，agent 表现得也就越高效。

有点类似在一些搜索问题中的 heuristics。

most common design for this function is a linear combination of **features**：

- Eval(s) = w1 f1(s) + w2 f2(s) + ... + wn fn(s)

Each fi(s) corresponds to a feature extracted from the input state s, and each feature is assigned a corresponding **weight** wi.

**Features** are simply some element of a game state that we can extract and assign a numerical value.

当然，Evaluation Function 也可以十分自由，没必要非得 linear。

基于 networks 的 nonlinear 的 evaluation function 在强化学习的应用中也十分广泛。

> **The most important thing to keep in mind is that the evaluation function yields higher scores for better positions as frequently as possible.** {: .prompt-tip }

