---
title: CS229-Linear Regression and Gradient Descent Lecture2
author: Sillycheese
date: 2024/5/18 11:49:51 +0800
categories:
  - CS229
tags:
  - ML
---

## Linear Regression

选择参数是 learning algorithm 里很重要的一部分。

To perform supervised learning, we must decide how we’re going to represent functions/hypotheses h in a computer. As an initial choice, let’s say we decide to approximate y as a linear function of x
$$
hθ(x) = θ₀ + θ₁x₁ + θ₂x₂
$$
θ 就是参数，也被称为权重。
$$
h(x) = \sum_{i = 0}^{d} \theta_i x_i = \theta^T x
$$

Now, given a training set, how do we pick, or learn, the parameters θ? To formalize this, we will define a function that measures, for each value of the θ’s, how close the h(x(i))’s are to the corresponding y(i)’s.

没错，就是 **cost function**：
$$
J(\theta) = \frac{1}{2} \sum_{i = 1}^{n} (h_{\theta}(x^{(i)}) - y^{(i)})^2
$$
与最小二乘的 cost function 有些熟悉，让我们继续。

### LMS  algorithm

We want to choose θ so as to **minimize** J(θ)。

Specifically, let’s consider the gradient descent algorithm, which starts with some initial θ, and repeatedly performs the update:
$$
\theta_j := \theta_j - \alpha \frac{\partial}{\partial \theta_j} J(\theta)
$$
α is called the **learning rate**.This is a very natural algorithm that repeatedly takes a step in the direction of steepest decrease of J.
$$
\begin{align*}
\frac{\partial}{\partial \theta_j} J(\theta) &= \frac{\partial}{\partial \theta_j} \frac{1}{2} (h_{\theta}(x) - y)^2 \\
&= 2 \cdot \frac{1}{2} (h_{\theta}(x) - y) \cdot \frac{\partial}{\partial \theta_j} (h_{\theta}(x) - y) \\
&= (h_{\theta}(x) - y) \cdot \frac{\partial}{\partial \theta_j} \left( \sum_{i = 0}^{d} \theta_i x_i - y \right) \\
&= (h_{\theta}(x) - y) x_j
\end{align*}
$$
For a single training example, this gives the update rule:
$$
\theta_j := \theta_j + \alpha (y^{(i)} - h_{\theta}(x^{(i)})) x^{(i)}_j
$$
The rule is called the **LMS** update rule (LMS stands for “least mean squares”), and is also known as the **Widrow-Hoff** learning rule.

magnitude of update 与 **error term**  $(y^{(i)} - h_{\theta}(x^{(i)}))$

有更大的 error 将会对参数发生巨大改变。

有两种方法来修改这个方法对于多于一个 example 的训练集。第一种便是上面的 LMS update rule。

第二种是：
$$
\theta := \theta + \alpha \sum_{i = 1}^{n} (y^{(i)} - h_{\theta}(x^{(i)})) x
$$

This method looks at every example in the entire training set on every step, and is called **batch gradient descent**.

$$
\theta := \theta + \alpha (y^{(i)} - h_{\theta}(x^{(i)})) x^{(i)}
$$
在这个算法中，我们更新参数只根据error相对于单个的example。This algorithm is called **stochastic gradient descent** (also **incremental gradient descent**).

> Often, stochastic gradient descent gets θ “close” to the minimum much faster than batch gradient descent.  {: .prompt-tip }

