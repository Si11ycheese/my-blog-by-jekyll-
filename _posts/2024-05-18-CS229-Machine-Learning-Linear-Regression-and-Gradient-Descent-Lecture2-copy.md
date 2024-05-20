---
title: CS229-Linear Regression and Gradient Descent Lecture2
author: Sillycheese
date: 2024/5/18 11:49:51 +0800
math: true
categories:
  - CS229
tags:
  - ML
---

## Linear Regression

选择参数是 learning algorithm 里很重要的一部分。

To perform supervised learning, we must decide how we’re going to represent functions/hypotheses h in a computer. As an initial choice, let’s say we decide to approximate y as a linear function of x

$$
h_{\theta}(x) = \theta_{0} + \theta_{1}x_{1} + \theta_{2}x_{2}
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
\theta_j := \theta_j - \alpha \frac{\partial}{\partial \theta_j}J(\theta)
$$

α is called the **learning rate**.This is a very natural algorithm that repeatedly takes a step in the direction of steepest decrease of J.

$$
\begin{align*}
 \frac{\partial}{\partial \theta_j} J(\theta) &= \frac{\partial}{\partial  \theta_j} \frac{1}{2} (h_{\theta}(x) - y)^2 \\
 &= 2 \cdot \frac{1}{2} (h_{\theta}(x) - y) \cdot \frac{\partial}{\partial  \theta_j} (h_{\theta}(x) - y) \\
 &= (h_{\theta}(x) - y) \cdot \frac{\partial}{\partial \theta_j} \left(  \sum_{i = 0}^{d} \theta_i x_i - y \right) \\
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

在这个算法中，我们更新参数只根据 error 相对于单个的 example。This algorithm is called **stochastic gradient descent** (also **incremental gradient descent**).

> Often, stochastic gradient descent gets θ “close” to the minimum much faster than batch gradient descent. 
{: .prompt-tip }

### Least squares revisited

因为$h_{\theta}(x^{(i)}) = (x^{(i)})^T \theta$​,我们可以轻易地用矩阵表示


$$
X\theta - \vec{y} = 
\begin{bmatrix}
(x^{(1)})^T \theta \\
\vdots \\
(x^{(n)})^T \theta 
\end{bmatrix}
-
\begin{bmatrix}
y^{(1)} \\
\vdots \\
y^{(n)} 
\end{bmatrix}
=
\begin{bmatrix}
h_{\theta}(x^{(1)}) - y^{(1)} \\
\vdots \\
h_{\theta}(x^{(n)}) - y^{(n)}
\end{bmatrix}
$$


同时，用这个公式$z^T z = \sum_i z_i^2$​，我们能进一步推


$$
\frac{1}{2}(X\theta - \vec{y})^T (X\theta - \vec{y}) = \frac{1}{2} \sum_{i=1}^{n} (h_{\theta}(x^{(i)}) - y^{(i)})^2 = J(\theta)
$$


最后，为了**minimize** J，让我们进一步推出它的梯度

$$
\begin{align*}
\nabla_{\theta} J(\theta) &= \nabla_{\theta} \frac{1}{2} (X\theta - \vec{y})^T (X\theta - \vec{y}) \\
&= \frac{1}{2} \nabla_{\theta} \left((X\theta)^T X\theta - (X\theta)^T \vec{y} - \vec{y}^T (X\theta) + \vec{y}^T \vec{y}\right) \\
&= \frac{1}{2} \nabla_{\theta} \left(\theta^T (X^T X) \theta - \vec{y}^T (X\theta) - \vec{y}^T (X\theta)\right) \\
&= \frac{1}{2} \nabla_{\theta} \left(\theta^T (X^T X) \theta - 2 (X^T \vec{y})^T \theta\right) \\
&= \frac{1}{2} \left(2 X^T X \theta - 2 X^T \vec{y}\right) \\
&= X^T X \theta - X^T \vec{y}
\end{align*}
$$

为了**minimize** J,我们将它的梯度设置成0，因此得到**normal equations**


$$
X^T X \theta = X^T \vec{y}
$$


同时，得到θ的值
$$
\theta = ({X^T X})^{-1}X^T \vec{y}
$$



### Probabilistic interpretation

In this section,你将看到一系列的probabilistic assumptions，在这之下，**least-squares regression**将会十分自然的衍生出来。


$$
y^{(i)} = \theta^T x^{(i)} + \epsilon^{(i)}
$$


在这里面，$\epsilon^{(i)}$​是error(unmodeled effects or random noise)。我们可以用正态分布来表示这个assumption。


$$
p(\epsilon^{(i)}) = \frac{1}{\sqrt{2\pi}\sigma} \exp \left( - \frac{(\epsilon^{(i)})^2}{2\sigma^2} \right)
$$


这意味着


$$
p(y^{(i)}|x^{(i)};\theta) = \frac{1}{\sqrt{2\pi}\sigma} \exp \left( - \frac{(y^{(i)}-\theta^{T}x^{(i)})^2}{2\sigma^2} \right)
$$


注意：$\theta$不是一个随机变量

因此，根据上面的assumption，我们可以进一步推出$p(\vec{y}|X;\theta)$,我们会将它称为**likelihood** function


$$
L(\theta)=L(\theta;X,\vec{y})=p(\vec{y}|X;\theta)
$$


根据独立性，这也可以被写为


$$
L(\theta) = \prod_{i=1}^{n} p(y^{(i)} | x^{(i)}; \theta) = \prod_{i=1}^{n} \frac{1}{\sqrt{2\pi}\sigma} \exp \left( - \frac{(y^{(i)} - \theta^T x^{(i)})^2}{2\sigma^2} \right)
$$


对于我们如何选择最好的参数，我们可以用**maximum likelihood**法则，也就是选择一个参数来使得data的概率越高越好。

也就是，我们要挑选一个参数来**maximize** $L(\theta)$

我们可以maximize任意strictly increasing function of $L(\theta)$。比如，我们可以使用 **log likelihood** 来替代。


$$
\begin{align*}
\log L(\theta) &= \log \left( \prod_{i=1}^{n} \frac{1}{\sqrt{2\pi}\sigma} \exp \left( - \frac{(y^{(i)} - \theta^T x^{(i)})^2}{2\sigma^2} \right) \right) \\
&= \sum_{i=1}^{n} \log \left( \frac{1}{\sqrt{2\pi}\sigma} \exp \left( - \frac{(y^{(i)} - \theta^T x^{(i)})^2}{2\sigma^2} \right) \right) \\
&= \sum_{i=1}^{n} \left( \log \left( \frac{1}{\sqrt{2\pi}\sigma} \right) - \frac{1}{\sigma^2} \cdot \frac{1}{2} (y^{(i)} - \theta^T x^{(i)})^2 \right) \\
&= n \log \left( \frac{1}{\sqrt{2\pi}\sigma} \right) - \frac{1}{\sigma^2} \cdot \frac{1}{2} \sum_{i=1}^{n} (y^{(i)} - \theta^T x^{(i)})^2
\end{align*}
$$


所以，我们要maximize$L(\theta)$只需要minimize:


$$
\frac{1}{2} \sum_{i=1}^{n} (y^{(i)} - \theta^T x^{(i)})^2
$$


这就是被称为$J(\theta)$的函数，也就是我们最初的least squares cost function.

注意，我们对于参数的选择不取决于$\sigma^2$,且事实上我们可能得到相同的结果即使$\sigma^2$是未知的

