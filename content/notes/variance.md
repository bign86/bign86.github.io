---
title: Variance
description: ""
date: 2020-11-24
preview: ""
draft: false
tags:
    - statistics
categories:
    - math
    - notes
math: true
---

The **Variance \(\text{Var}[X],\:\sigma^2\)** is the second _central_ moment, a measure of dispersion, and is defined as the expected value of the squared deviation from its expected value

$$\text{Var}[X] = E\left[\left(X-E[X]\right)^2\right] = E\left[X^2\right] - E\left[X\right]^2$$

The variance corresponds to the [covariance](/notes/covariance) of a random variable with itself.
For discrete variables it is calculated as

$$\text{Var}[X] = \sum_i^n p_i\:(x_i - \mu)^2 \qquad\text{with}\quad\mu = \frac{1}{n}\sum_i^n x_i$$

while for continuous variables

$$\begin{align*}\text{Var}[X] &= \int_\mathbb{R} (x - \mu)^2\:f(x)\:dx \qquad\text{with}\quad\mu = \int_i^n x\:f(x)\:dx\\
&= \int_\mathbb{R} x^2\:dF(x) - \mu^2\end{align*}$$

> The proof is simple:
> $$\begin{align*}\text{Var}[X] &= \int_\mathbb{R} (x - \mu)^2\:f(x)\:dx\\
&= \int_\mathbb{R} x^2\:f(x)\:dx + \mu^2\int_\mathbb{R}f(x)\:dx - 2\mu\int_\mathbb{R} x\:f(x)\:dx\\
&= \int_\mathbb{R} x^2\:dF(x) + \mu^2 - 2\mu^2\\
&= \int_\mathbb{R} x^2\:dF(x) - \mu^2\end{align*}$$

### Properties

1. **Non-negativity:** follows from the fact that squares cannot be negative
  $$\text{Var}[X]\geq 0$$

2. **Constant:**
  $$\text{Var}[\alpha] = 0$$

3. **Finiteness:** the variance is infinite if the expectation is infinite, as in the case of the Cauchy distribution
  $$\text{if}\:\:E[X] = \infty\quad\Rightarrow\quad\text{Var}[X] = \infty$$
  However, it may at times be that the variabce is infinite even in the case of finite expectation, as in the case of the Pareto distribution.

4. **Invariance:** the variance is invariant wrt rigid shifts
  $$\text{Var}[X + \alpha] = \text{Var}[X]$$

5. **Scaling:** the variance scales with the square of the random variable
  $$\text{Var}[\alpha X] = \alpha^2\text{Var}[X]$$

### Conditional variance

The conditional variance between the random variables \(X\) and \(Y\) is given by

$$\text{Var}[X\vert Y] = E\left[\left(X-E(X\vert Y)\right)^2\vert Y\right]$$

For discrete variables

$$\text{Var}[X\vert Y=y] = E\left[\left(X-E(X\vert Y=y)\right)^2\vert Y=y\right]$$

while for continuous variables

$$\text{Var}[X\vert Y=y] = \int_\mathbb{R}\left(x - \int_\mathbb{R} x'\:f_{X\vert Y}\left(dx'\vert y\right)\right)^2\:f_{X\vert Y}\left(dx\vert y\right)$$

### Variance of the sum

Given \(n\) random variables \(\{X_i\}\), the variance of their sum can be written as

$$\begin{align*}\text{Var}\left[\sum_i \alpha_i X_i\right] &= \sum_{i,j} \alpha_i\alpha_j\:\text{Cov}\left[X_i,X_j\right]\\
 &= \sum_i \alpha_i^2\:\text{Var}[X_i] + \sum_{i\neq j} \alpha_i\alpha_j\:\text{Cov}\left[X_i,X_j\right]\end{align*}$$

If the variables are all independent of each other, then they are also **uncorrelated**. Therefore it is true that

$$\text{if}\:X_i\perp\!\!\!\perp X_j\:\:\forall\:i,j\quad\Rightarrow\quad\rho[X_i,X_j]=0\quad\Rightarrow\quad\text{Cov}[X_i,X_j]=0$$

and the variance of the sum becomes the simple sum of the variances

$$\text{Var}\left[\sum_i \alpha_i X_i\right] = \sum_i \alpha_i^2\:\text{Var}[X_i]$$

_Note_ that the opposite is NOT true. Independence is sufficient to determine that variables are uncorrelated. However, since it is not _necessary_, is it possible for uncorrelated variables to be dependent.

> The proof of the formula for the variance of uncorrelated variables follows from the definition of variance
> $$\begin{align*}
\text{Var}\left[X+Y\right] &= E\left[\left(X+Y\right)^2\right] - E\left[X+Y\right]^2\\
&= E\left[X^2+2XY+Y^2\right] - E\left[E\left[X\right]+E\left[Y\right]\right]^2\\
&= E\left[X^2\right]+2E\left[XY\right]+E\left[Y^2\right] - E\left[X\right]^2-2E\left[X\right]E\left[Y\right]-E\left[Y\right]^2 &\text{(*1)}\\
&= E\left[X^2\right] - E\left[X\right]^2 + E\left[Y^2\right] - E\left[Y\right]^2\\
&= \text{Var}\left[X\right] + \text{Var}\left[Y\right]
\end{align*}$$
> where in \((*1)\) we used the linearity of the expectation and the independence.

#### Bienayme formula

If all variables are independent and have equal variance \(\sigma^2\), then

$$\begin{align*}\text{Var}\left[\langle X\rangle\right] &= \text{Var}\left[\frac{1}{n}\sum_i X_i\right]\\ &= \frac{1}{n^2}\sum_i\text{Var}\left[X_i\right]\\ &= \frac{\sigma^2}{n}\end{align*}$$




