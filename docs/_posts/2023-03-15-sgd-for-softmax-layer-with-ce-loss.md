---
layout: post
title:  "Stochastic Gradient Descent for Softmax activation and Cross Entropy Loss"
date:   2023-03-15 11:08:12 +0800
categories: jekyll update
tags: gradient-descent deep-learning
description: Derive the partial derivative of Cross Entropy loss to neural network last layer weights.
---

Classification is used to predict discrete class labels. In deep learning, we often use the Softmax activation function in the final layer to convert logits to probabilities of the input belonging to the classes. The most common method to compute loss for classification task is Cross Entropy (CE) loss. This post explains how loss is back propagated to the **last layer's weight** using Stochastic Gradient Descent (SGD).

Consider neural network $f$ that has $K$ outputs and uses Softmax function $\sigma$ for the last layer activation. $f$ is parameterized by $\theta = (W, V)$, where $W$ are the weights of the last layer, and $V$ consists of weights of all previous layers. $f(x; \theta) = \sigma(W \cdot z(x;V))$, where $z$ is the nonlinear function that maps an input $x$ to the output of the network's penultimate layer. $y$ is the ground truth label for $x$. We want to find the partial derivative of CE loss ($L_{CE}$) to $W$.

$$ u = W \cdot z(x; V) = (u_1 \cdots u_K) = (w_1 \cdot z(x; V) \cdots w_K \cdot z(x; V))$$

$$ u_k = w_k \cdot z(x; V) $$

$$ p_y = \sigma (u)_y = \frac{e^{u_y}}{\sum_{k\prime = 1}^{K} e^{u_{k\prime}}} $$

$$ L_{CE} = - \log (p_y) = - \log (\sigma(u)_y) $$

The partial derivative of $L_{CE}$ to $W$ is a matrix consisting of partial derivative of $L_{CE}$ to $W_k$:

$$ \frac{\partial L_{CE}}{\partial W} = (\frac{\partial L_{CE}}{\partial w_1} \cdots \frac{\partial L_{CE}}{\partial w_K}) $$

$\frac{\partial L_{CE}}{\partial w_k}$ is computed by combining 3 partial derivatives using chain rule. We can obtain the partial derivatives using 3 steps.

$$ \frac{\partial L_{CE}}{\partial w_k} = \frac{\partial L_{CE}}{\partial p_y} \cdot \frac{\partial p_y}{\partial u_k} \cdot \frac{\partial u_k}{\partial w_k} $$

# Step 1: $\frac{\partial L_{CE}}{\partial p_y}$

$$
\begin{align}
\frac{\partial L_{CE}}{\partial p_y} &= \frac{\partial - \log (p_y)}{\partial p_y} \\
&= - \frac{1}{p_y} 
\end{align}
$$

# Step 2: $\frac{\partial p_y}{\partial u_k}$

$$
\begin{align}
\frac{\partial p_y}{\partial u_k} &= \frac{\partial \sigma(u)_y}{\partial u_k}
\end{align}
$$

If $k = y$:

$$
\begin{align}
\frac{\partial \sigma(u)_y}{\partial u_k} &= \frac{e^{u_k} \sum_{k \prime=1}^K e^{u_{k\prime}} - e^{u_k} e^{u_k}}{(\sum_{k \prime=1}^K e^{u_{k\prime}})^2} \\
&= \frac{e^{u_k}}{\sum_{k\prime = 1}^{K} e^{u_{k\prime}}} \frac{\sum_{k\prime=1}^{K} e^{u_{k\prime}} - e^{u_k}}{\sum_{k\prime=1}^{K} e^{u_{k\prime}}} \\
&= \frac{e^{u_k}}{\sum_{k\prime = 1}^{K} e^{u_{k\prime}}} (\frac{\sum_{k\prime=1}^{K} e^{u_{k\prime}}}{\sum_{k\prime=1}^{K} e^{u_{k\prime}}} - \frac{e^{u_k}}{\sum_{k\prime=1}^{K} e^{u_{k\prime}}}) \\
&= p_k (1 - p_k) \\
&= p_y (1 - p_k)
\end{align}
$$

If $k \neq y$

$$
\begin{align}
\frac{\partial \sigma(u)_y}{\partial u_k} &= \frac{0 \sum_{k \prime=1}^K e^{u_{k\prime}} - e^{u_k} e^{u_y}}{(\sum_{k \prime=1}^K e^{u_{k\prime}})^2} \\
&= - \frac{e^{u_k} e^{u_y}}{(\sum_{k \prime=1}^K e^{u_{k\prime}})^2} \\
&= - p_k p_y \\
&= p_y (0 - p_k)
\end{align}
$$

Combining the above cases, $\frac{\partial p_y}{\partial u_k} = p_y (1(k = y) - p_k)$.

# Step 3: $\frac{\partial u_k}{\partial w_k}$

Since $u_k = w_k \cdot z(x; V)$,

$$
\frac{\partial u_k}{\partial w_k} = z(x; V)
$$

# Final $\frac{\partial L_{CE}}{\partial W}$ and $\frac{\partial L_{CE}}{\partial w_k}$

$$
\begin{align}
\frac{\partial L_{CE}}{\partial w_k} &= \frac{\partial L_{CE}}{\partial p_y} \cdot \frac{\partial p_y}{\partial u_k} \cdot \frac{\partial u_k}{\partial w_k} \\

&= - \frac{1}{p_y} \cdot p_y (1(k=y) - p_k) \cdot z(x; V) \\

&= (p_k - 1(k=y)) \cdot z(x; V)
\end{align}
$$

$\frac{\partial L_{CE}}{\partial W}$ matrix can be formed using $\frac{\partial L_{CE}}{\partial w_k}$ vectors.

$\frac{\partial L_{CE}}{\partial W}$ is used to update $W$ during SGD learning using $W \leftarrow W - \alpha \frac{\partial L_{CE}}{\partial W}$, where $\alpha$ is the learning rate.
