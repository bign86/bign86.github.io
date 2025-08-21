---
title: Expected value
description: ""
date: 2024-11-24
preview: ""
draft: false
tags: [statistics]
categories: [math, notes]
math: true
---

The **Expected value \(E[X],\:\mu\)** is the first _raw_ moment and a generalization of the weighted average. Is the mean of the possible values of the random variable \(X\). Note that the expected value may not be one possible outcome from the sample space. It is not therefore, the value one would "expect" to obtain from a trial in the strict sense.

The expected value is the raw moment \(\mu'_n\) where \(n=1\)

$$\mu'_1 = E\left[X\right] = \begin{cases}\sum_i x_i p_i & \text{Discrete variables}\\\int_{\mathbb{R}} x f(x)\:dx & \text{Continuous variables}\end{cases}$$

#### Events

The case for a finite set of \(n\) events coincides with the definition of a weighted average, that becomes an arithmetic average if all outcomes are equally likely. The definition is formally the same also in the case of a _countably infinite set_ of outcomes where \(n=\infty\)

$$E[X]=\sum_{i=1}^\infty x_i p_i$$

For \(E[X]\) to be defined in this case, the infinite summation must converge absolutely. Depending on the maximum and minimum expected value, the following cases can be found:

$$\text{with}\quad\begin{cases}E[X^+]=E\left[\text{max}\left\{X,0\right\}\right]\\E[X^-]=E\left[-\text{min}\left\{X,0\right\}\right]\end{cases}\qquad E[X]=\begin{cases}E[X^+]-E[X^-] & E[X^+]< \infty\:\land\:E[X^-]< \infty\\\infty & E[X^+]=\infty\:\land\:E[X^-]< \infty\\-\infty & E[X^+]< \infty\:\land\:E[X^-]=\infty\\\text{undefined} & E[X^+]=\infty\:\land\:E[X^-]=\infty\\\end{cases}$$

However, in some cases of infinite summations, the value of the sum depends on the order of the summands. Since random variables are not ordered, the formula above may not be suitable as a formal definition of expected value in this case.

#### Continuous variables

Whenever the random variable \(X\) admits a density \(f\), the expected value is given by

$$E[X]=\int_\mathbb{R} x\:f(x)\:dx$$

The expected value can be defined also on the CDF \(F(x)\):

$$\begin{gather*}E[X]=\mu\in\mathbb{R}\quad\iff\quad\int_{-\infty}^\mu F(x)\:dx=\int_\mu^\infty\left(1-F(x)\right)\:dx\quad\text{w/ both integrals converging}\\\Longrightarrow\quad E[x]=\int_0^\infty\left(1-F(x)\right)\:dx - \int_{-\infty}^0 F(x)\:dx\end{gather*}$$

### Properties of the expected value

There are several properties followed by the expected value for the most deriving from its definition as a Lebesgue integral.

1. **Equal variables:**
  $$\text{if}\:\:X=Y\:\:\text{a.s.}\quad\Rightarrow\quad E[X]=E[Y]$$

2. **Constant:** with \(c\in\mathbb{R}\) constant
  $$\text{if}\:\:X=c\:\:\text{a.s.}\quad\Rightarrow\quad E[X]=c$$
  A consequence of this is that, being \(E[X]=\mu\) a constant
  $$E\left[E[X]\right]=E[X]$$

3. **Non-negativity:**
  $$\text{if}\:\:X\geq 0\:\:\text{a.s.}\quad\Rightarrow\quad E[X]\geq 0$$

4. **Linearity:** for any random variable \(X\) and \(Y\) and constant \(c\)
  $$\begin{align*}E[X+Y]&=E[X]+E[Y]\\E[cX]&=cE[X]\end{align*}$$
  consequently by induction the property can be generalized as
  $$E\left[\sum_i^N c_iX_i\right]=\sum_i^N c_iE[X_i]$$

5. **Monotonicity:** for any random variable \(X\) and \(Y\)
  $$\text{if}\:\:X\leq Y\:\:\text{a.s.},\:\:\exists\:E[X],\:\:\exists\:E[Y]\quad\Rightarrow\quad E[X]\leq E[Y]$$

6. **Non-degeneracy:**
  $$\text{if}\:\:E\left[\vert X\vert\right]=0\quad\Rightarrow\quad X=0\:\:\text{a.s.}$$

7. **Absolute value:** it can be proved that
  $$\vert E[X]\vert\leq E[X]$$

8. **Non-multiplicativity:** linearity in the product requires _independence_ of \(X\) and \(Y\)
  $$X\perp\!\!\!\perp Y\qquad\Rightarrow\qquad E[X\:Y]=E[X]\:E[Y]$$

### Important inequalities

There are a number of important inequalities related to the expected value. Here are collected in brief the most important ones without proof.

1. **Markov inequality:**
  $$\text{with}\:\:X\geq 0,\:c > 0,\quad P(X\geq c)\leq\frac{E[X]}{c}$$

2. **Chebyshev's inequality:** applying the Markov inequality to \(\vert X-E[X]\vert^2\), it can be proved that
  $$\text{with}\:\:X\geq 0,\:c > 0,\quad P\left(\vert X-E[X]\vert\geq c\right)\leq\frac{\text{Var}[X]}{c^2}$$

3. **Jensen inequality:** with \(f\::\:\mathbb{R}\rightarrow\mathbb{R}\) _convex_ function and \(X\) random variable with finite expectation \(E[X]<\infty\)
  $$f\left(E[X]\right)\leq E\left[f(X)\right]$$

4. **Hölder inequality:**
  $$\text{if}\:\:p > 1,\:q > 1\:\:\text{with}\:\:p^{-1}+q^{-1}=1\qquad\Longrightarrow\qquad E\left[\vert X\:Y\vert\right]\leq\left(E\left[\vert X\vert^p\right]\right)^{\frac{1}{p}}\:\left(E\left[\vert Y\vert^q\right]\right)^{\frac{1}{q}}$$

5. **Cauchy-Schwarz inequality:** very important special case of the Hölder inequality for \(p=q=2\)
  $$\left\vert E[X\:Y]\right\vert\leq\left(E\left[X^2\right]\right)^{\frac{1}{2}}\:\left(E\left[Y^2\right]\right)^{\frac{1}{2}}$$

## Conditional expected value

The conditional expected value \(E\left[X\vert Y\right]\) is defined as

$$E\left[X\vert Y\right] = \sum_x x\:P\left(X=x\vert Y=y\right) = \sum_x x\:\frac{P\left(X=x, Y=y\right)}{P\left(Y=y\right)}\quad\text{discrete variables}$$
$$E\left[X\vert Y\right] = \int_\mathbb{R} x\cdot f_{X\vert Y}\left(x\vert y\right)\:dx = \frac{1}{f_{Y}\left(y\right)}\int_\mathbb{R} x\cdot f_{X,Y}\left(x, y\right)\:dx\quad\text{continuous variables}$$

### Properties of the conditional expected value

1. **Independence:**
  $$\text{if}\:\:X \perp\!\!\!\perp Y\quad\Rightarrow\quad E[X\vert Y]=E[X]$$

2. **Ordering:**
  $$\text{if}\:\:X \leq Y\quad\Rightarrow\quad E[XY]\leq E[Y]$$

3. **Law of total expectation:**\
  Follows from the law pf total probability.

## Proofs of properties

- **Non-negativity:**
  > The proof follows from the definition of expected value noticing that probabilities are non-negative.

- **Linearity:**
  > The proof is identical for discrete and continuous variables. Starting with the discrete variables:
  $$\begin{align*}
  E\left[a\:X+b\:Y\right] &= \sum_{x\in X} \sum_{y\in Y} (ax+by)P\left(X=x,Y=y\right)\\
  &= \sum_{x\in X}\sum_{y\in Y} ax\:P\left(X=x,Y=y\right) + \sum_{x\in X}\sum_{y\in Y} by\:P\left(X=x,Y=y\right)\\
  &= a\:\sum_{x\in X}x\:\sum_{y\in Y} P\left(X=x,Y=y\right) + b\:\sum_{y\in Y} y\:\sum_{x\in X} P\left(X=x,Y=y\right)\\
  &= a\:\sum_{x\in X}x\:P\left(X=x\right) + b\:\sum_{y\in Y} y\:P\left(Y=y\right)\\
  &= a\:E\left[X\right] + b\:E\left[Y\right]\\
  \end{align*}$$
  For continuous variables
  $$\begin{align*}
  E\left[a\:X+b\:Y\right] &= \int_{X} \int_{y\in Y} (ax+by)\cdot f_{X,Y}\left(x,y\right)\:dx\:dy\\
  &= \int_{X}\int_{Y} ax\cdot f_{X,Y}\left(x,y\right)\:dx\:dy + \int_{X}\int_{Y} by\cdot f_{X,Y}\left(x,y\right)\:dx\:dy\\
  &= a\:\int_{X}x\:\int_{Y} f_{X,Y}\left(x,y\right)\:dx\:dy + b\:\int_{Y} y\:\int_{X} f_{X,Y}\left(x,y\right)\:dx\:dy\\
  &= a\:\int_{X}x\cdot f_{X}\left(x\right)\:dx + b\:\int_{Y} y\cdot f_{Y}\left(y\right)\:dy\\
  &= a\:E\left[X\right] + b\:E\left[Y\right]\\
  \end{align*}$$

- **Monotonicity:**
  > Let \(Z=Y-X\), then
  $$E\left[Z\right] = E\left[Y-X\right]= E\left[Y\right] - E\left[X\right]\quad\text{(linearity)}$$
  and
  $$\text{if}\quad Z\geq 0\quad\Rightarrow\quad E\left[Z\right]\geq 0\quad\text{(non-negativity)}$$
  Equating the two conditions
  $$E\left[Z\right] = E\left[Y\right] - E\left[X\right] \geq 0 \quad\Rightarrow\quad E\left[X\right] \geq E\left[Y\right]$$

- **Non-multiplicativity:**
  > Assume \(X\) and \(Y\) independent. Then, for discrete variables
  $$\begin{align*}
  E\left[X\:Y\right] &= \sum_{x\in X} \sum_{y\in Y} (x\cdot y)P\left(X=x,Y=y\right)\\
  &= \sum_{x\in X} \sum_{y\in Y} (x\cdot y)\left(P(X=x)\cdot P(Y=y)\right) &\text{(independence)}\\
  &= \sum_{x\in X} x\:P(X=x)\:\sum_{y\in Y} y\:P(Y=y)\\
  &= E\left[X\right]\:E\left[Y\right]
  \end{align*}$$
  For continuous variables
  $$\begin{align*}
  E\left[X\:Y\right] &= \int_{x} \int_{y} (x\cdot y)f_{X,Y}\left(x,y\right)\\
  &= \int_{x} \int_{y} (x\cdot y)\left(f_{X}\left(x\right)\cdot f_{Y}\left(y\right)\right) &\text{(independence)}\\
  &= \int_{x} x\cdot f_{X}\left(x\right)\:\int_{y} y\cdot f_{Y}\left(y\right)\\
  &= E\left[X\right]\:E\left[Y\right]
  \end{align*}$$
