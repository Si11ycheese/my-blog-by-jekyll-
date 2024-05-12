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

- States:S(start at s0)
- Players:一组经常轮流的玩家
- Actions：取决于玩家/状态
- Transition Function：很像常规搜索的后继函数（state and action to the next state）
- Terminal Test：游戏是否结束
- Terminal Utilities：获胜的结果（会被记录）
- Solution：也就是策略，每种可能的情况应该采取的策略
