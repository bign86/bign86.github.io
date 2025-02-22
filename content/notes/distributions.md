---
title: Distributions
description: ""
date: 2024-12-08
preview: ""
draft: false
tags: [statistics]
categories: [math, notes]
math: true
---

The **probability distribution** \(P\) is the mathematical function that calculates the likelyhood of an event from an experiment.
$$P(A\subseteq\Omega)\;\rightarrow\;[0, 1]\in\mathbb{R}$$

The **support** of the distribution is the subset of the function domain \(X\) having non-zero probability
$$\text{with}\:f:X\rightarrow\mathbb{R},\quad\text{supp}(f)=\{x\in X:f(x)\neq 0\}$$

Probability distributions may be defined for discrete or continuous random variables throught the _Cumulative Distribution Function (CDF)_, the _Probability Mass Function (PMF)_ (for discrete variables), and the _Probability Density Function (PDF)_ (for continuous variables).

### Cumulative distribution function

The CDF \(F_X\) is defined as

$$F:\mathbb{R}\:\rightarrow\:[0,1]\qquad F_X(x)=P(X\leq x)=\begin{cases}\sum_{x_i\leq x}P(X=x_i)&\text{discrete variables}\\\int_{-\infty}^x f_X(t)\:dt&\text{continuous variables}\end{cases}$$

where \(f_X(x)\) is the PDF defined below.\
There are few properties:

- The CDF is right-continuous and monotone increasing
- The CDF goes to 0 on the left, and to 1 on the right
  $$\lim_{x\to-\infty}F_X(x)=0\quad\text{and}\quad\lim_{x\to\infty}F_X(x)=1$$

La _Survival function_ \(S_X\) is defined as the complementary of the CDF

$$P(X>x)=1-F_X(x)=S_X(x)$$

The multivariate form of the CDF is given by

$$F_{X_1,\ldots,X_n}(x_1,\ldots,x_n)=P(X_1\leq x_1,\ldots,X_n\leq x_n)$$

The marginal CDF for \(X\) and \(Y\) is given by

$$F_{X,Y}(x,y)=\int_{x_0}^{x_T}\int_{y_0}^{y_T}f(x,y)\:dx\:dy$$

### Probability mass function

The PMF gives the probability that a _discrete_ random variable is exactly equal to a value

$$p:\mathbb{R}\:\rightarrow\:[0,1]\qquad p_X(x)=P(X=x)\qquad\text{with }-\infty < x < \infty$$

The PMF fulfills:

- The PMF is always positive
  $$p_X(x)\geq 0$$
- The PMF sums up to 1
  $$\sum_x p_X(x)=1$$

The multivariate form of the PMF is given by

$$\begin{gather*}p_{X_1,\ldots,X_n}(x_1,\ldots,x_n)=P(X_1=x_1,\ldots,X_n=x_n)\\\text{with}\quad\sum_{i,j,\ldots,k}P(X_1=x_{1i},X_2=x_{2i},\ldots,X_n=x_{nk})=1\end{gather*}$$

The marginal PMF for \(X\), given \(X\) and \(Y\), is

$$p_X(x)=E_Y\left[p_{X\vert Y}(x,y)\right]$$

### Probability density function

The PDF gives the relative likelyhood that a _continuous_ random variable is exactly equal to a value. A random variable \(X\) has a density PDF \(f_X\), non-negative Lebesgue-integrable if

$$\Pr[a\leq X\leq b]=\int_a^b f_X(x)\:dx$$

If \(f_X\) is also continuous it can be obtained from the CDF

$$f_X(x)=\frac{d}{dx}F_X(x)$$

Considering an event \(E\) this can be understood as:

$$P(X\in E) = \int_E f(x)\:dx = \sum_{\omega\in\Omega} p(\omega)\int_E \delta(x - \omega) = \sum_{\omega\in\Omega\cap E} p(\omega)$$

The PDF fulfills:

- The PDF is always positive
  $$f_X(x)\geq 0$$
- The PDF sums up to 1
  $$\int_\mathbb{R} f_X(x)\:dx=1$$

The multivariate form of the PDF is given by

$$f_{X_1,\ldots,X_n}(x_1,\ldots,x_n) = \frac{\partial}{\partial x_1,\ldots,\partial x_n}F_{X_1,\ldots,X_n}(x_1,\ldots,x_n)$$

The marginal PDF for \(X\), given \(X\) and \(Y\), is

$$f_X(x)=\int_{y_0}^{y_T}f_{X,Y}(x,y)\:dy\qquad\text{with }y\in\left[y_0,y_T\right]$$

### Definitions related to distributions

The **q-quantile** \(x\) is the value such that
$$P(X < x)=q$$

The **median** is the value such that the probability of finding values greater or smaller than the median is 50%
$$m=\text{Median}(X)\::\:P(X < m)=P(X > m)=0.5$$

The **mode** is the most likely value from the sample space if discrete, the value at which the probability density function peaks if continuous.

## Notable discrete distributions

- [Binomial](/notes/binomial)
- [Poisson](/notes/poisson)
