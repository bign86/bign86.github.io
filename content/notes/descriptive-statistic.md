---
title: Descriptive statistic
description: ""
date: 2024-12-28
preview: ""
draft: false
tags: [statistics]
categories: [math, notes]
math: true
---

A descriptive statistic summarizes quantitatively some feature of a sample. Common descriptive statistics for distributions are _measures of central tendency_ and _measures of variability_.

### Measures of central tendency

Measures to find typical values (averages) of a distribution. The most common ones are:

- **Mode**: the most frequent value in the sample. Can be used with categorical data.
- **Median** [[link](/notes/median)]: is the value separating the higher half from the lower half of the sample ad corresponds to the 50th quantile. Is robust against outliers.
  $$P[X\leq m]\geq\frac{1}{2}\quad\text{and}\quad P[X\geq m]\geq\frac{1}{2}$$
- **Mean**
  - **Generalized mean**: defined as
    $$\left(\frac{1}{n}\sum_{i=1}^n x_i^p\right)^{1/p}$$
  - **Arithmetic mean**: generalized mean when \(p=1\)
    $$\frac{1}{n}\sum_i^n x_i$$
  - **Harmonic mean**: generalized mean when \(p=-1\)
    $$\frac{n}{\sum_{i=1}^n \frac{1}{x_i}}$$
  - **Geometric mean**: defined as
    $$\left(\prod_{i=1}^n x_i^p\right)^{1/n} = \exp\left(\frac{1}{n}\sum_{i=1}^n \ln x_i\right)$$

### Measures of scale

- **Variance**: for discrete variables defined as
  $$\sigma^2 := \sum_i^n p_i(x_i - \mu)^2$$
  In case of equally probable outcomes it simplifies into
  $$\sigma^2 := \frac{1}{n}\sum_i^n(x_i - \mu)^2$$
  For continuous variables is defined as
  $$\sigma^2 := \int_X (x - \mu)^2\:p(x)$$
  where \(p(x)\) is the probaility density function for the random variable \(X\). In both cases, \(\mu\) is the mean of the random variable.

- **Standard deviation**: the square root of the variance \(\sigma\).

- **Range**: narrowest interval containing all the data
 $$\text{Range} := \max[X] - \min[X]$$

- **Interquantile Range (IQR)**: distance between the 75th and the 25th percentiles of the data. The IQR is a trimmed estimator that can be used to identify outliers.

- **Mean absolute difference**: expected value of the absolute difference of the random variables \(X\) and \(Y\) i.i.d.
  $$MD := E\left[\vert X - Y\vert\right] = E_X\left[E_{X\vert Y}\left[\vert X - Y\vert\right]\right]=\begin{cases}\frac{1}{n^2}\sum_i^n\sum_j^n\vert x_i - y_i\vert & \text{discrete variables}\\\int_{-\infty}^{\infty}\int_{-\infty}^{\infty}f(x)f(y)\:\vert x - y\vert\:dx\:dy & \text{continuous variables}\end{cases}$$

- **Median absolute deviation (MAD)**: robust metric defined as the median of the absolute deviations from the median
  $$MAD := \text{Median}\left[\vert x_i - \text{Median}\left[x\right]\vert\right]$$

- **Average absolute deviation (AAD)**: defined as the mean of the absolute deviations from a central point.
  $$AAD := \text{Mean}\left[\vert x_i - \mu\left(x\right)\vert\right]$$
  The exact definition of AAD depends on the measure of central tendency \(\mu(\cdot)\) employed. Common options are the arithmetic mean, the median, the mode.

- **Coefficient of variation (CV)**: defined as the ratio between the standard deviationa nd the mean
  $$CV := \frac{\sigma}{\mu}$$
  Note that the CV has a meaning only for those variables having a natural definition of zero, not for those where the zero can be chosen arbitrarily.


https://en.wikipedia.org/wiki/Central_tendency\
https://en.wikipedia.org/wiki/Statistical_dispersion
