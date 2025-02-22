---
title: Binomial distribution
description: ""
date: 2024-12-08
preview: ""
draft: false
tags: [statistics]
categories: [math, notes]
math: true
---

The Binomial is the distribution followed by a binary variable in a sequence of \(n\) _independent_ experiments where the probability of success is \(p\). We define:

- _Bernoulli trial_: single experiment whose outcome is binary (success/failure).
- _Bernoulli process_: sequence (either finite or infinite) of Bernoulli trials. The trials must be _independent and identically distributed_ (\(iid\)). In other words is the sequence of independent random variables
  $$X_0, X_1, \ldots\quad\text{such that}\quad \begin{cases}X_i\in\{0,1\}\quad\forall i\\P(X_i=1)=\text{const}\quad\forall i\end{cases}$$

The process is **memoryless**, therefore:

- Past outcomes do not inform about future outcomes
- Each Bernoulli trial in a sequence can be understood as the origin of a Bernoulli process identical to the current one. This is called _fresh-start property_.

### Bernoulli distribution

The Bernoulli distribution describes the probability of having \(k\) successes in \(n\) Bernoulli trials (and hence \(n-k\) failures).

$$X\sim b(n,p)\quad\text{with}\quad n\in\mathbb{N}\quad\text{and}\quad p\in[0,1]$$

Its **probability mass function** is

$$\text{with}\quad\begin{cases}P(X=1)=p\\P(X=0)=1-p\end{cases}\quad\Rightarrow\quad P(X_1=x_1,\ldots,X_n=x_n)=p^k(1-p)^{n-k}$$

It can be desumed that the probability of obtaining any specific infinitely long sequence of outcomes has null probability. Therefore,

$$\lim_{k\rightarrow\infty}p^k=0\quad\forall\:0\leq p\leq 1$$

#### Moments

- The **expected value** equals the probability of success:
  $$E[X_i] = P(X_i=1)=p\quad \forall i$$
- The **variance**:
  $$\text{Var}[X]=p(1-p)$$
  > From the definition of variance in terms of expected value we get
  > $$\text{Var}[X_i]=E[X_i^2]-E[X_i]^2=p-p^2=p(1-p)$$

### Binomial distribution

The Binomial distribution describes the Bernoulli process, and is given by the combination of all the Bernoulli processes that will result in \(k\) successes in \(n\) trials, independently on the exact sequence of successes and failures in those sequences.

$$X\sim B(n,p)\quad\text{with}\quad n\in\mathbb{N}\quad\text{and}\quad p\in[0,1]$$

The **probability mass function** is defined as:

$$f(k,n,p)=P(X=k)=\binom{n}{k}p^k(1-p)^{n-k}$$

The second part of the rhs is the Bernoulli distribution, hence the probability of getting any sequence with \(k\) successes over \(n\) trials. The first part of the rhs, the binomial coefficient, represents the number of Bernoulli trials having \(k\) successes. This is needed as we are interested in all the possible sequences with the correct characteristics.

The **cumulative distribution function** is given by

$$F(k;n,p)=P(X\leq k)=\sum_i^{\lfloor k\rfloor}\binom{n}{i}p^i(1-p)^{n-i}$$

where \(\lfloor\cdot\rfloor\) denotes the floor function. The CDF can also be written in terms of the Incomplete Beta function \(B(a,b)\). Defining the _Regularized Incomplete Beta function_ as

$$I_x(a,b)=\frac{B(x;a,b)}{B(a,b)}$$

the CDF can be written as

$$\begin{align*}F(k;n,p) &= I_{1-p}(n-k,k+1)\\&=1-I_p(k+1,n-k)\\&=(n-k)\binom{n}{k}\int_0^{1-p}t^{n-k-1}(1-t)^k\:dt\end{align*}$$

#### Moments

- The **expected value** of the random variable \(X=\sum_i^n X_i\):
  $$E[X]=np$$
  > If \(X\) is i.i.d., applying the linearity of the expected value:
  > $$E[X]=E\left[\sum_i^n X_i\right]=\sum_i^n E[X_i]=\sum_i^n p=np$$
- The **variance**:
  $$\text{Var}[X]=np(1-p)$$
  > From the linearity of the random variable
  > $$\begin{align*}\text{Var}[X]&=\text{Var}\left[\sum_i^n X_i\right]\\&=\sum_i^n \text{Var}[X_i]&\text{linearity of the variance}\\&=\sum_i^n p(1-p)&\text{variance of the Bernoulli dist.}\\&=np(1-p)\end{align*}$$

#### Properties

- **Symmetry**: The distribution is symmetric around \(n/2\). Therefore, the values for \(k > n/2\) can be obtained from the values for \(k < n/2\)
  $$\Rightarrow\quad f(k,n,p) = f(n-k,n,1-p)$$
