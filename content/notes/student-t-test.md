---
title: Student's t-test
description: ""
date: 2024-12-28
preview: ""
draft: false
tags: [statistics]
categories: [math, notes]
math: true
---


The t-test compares the mean of the sample \(\langle X\rangle\) against the mean of a population (one-sample test), the mean of another sample (two-samples test), or zero (paired test), when the population variance \(\sigma\) is not known.

## t-statistic

In general, the t-statistic is the ratio between the estimated value and its standard error. Called \(\hat{\beta}\) the estimator of a parameter \(\beta\), the t-statistic for the parameter is a quantity

$$t_\hat{\beta} = \frac{\hat{\beta} - \beta_0}{\text{StdErr}(\hat{\beta})}$$

where \(\beta_0\) is the population value for \(\beta\), and \(\text{StdErr}[\cdot]\) is the _Standard Error_ function defined as the standard deviation of the estimator divided by the number of observations

$$\text{StdErr}(\hat{\beta}) = \frac{s}{\sqrt{n}}$$

where \(s\) is the sample standard deviation. The t-statistic is similar to the z-score but it is used when the sample sizes are small or the population standard deviation \(\sigma\) is unknown, hence the need to approximate it through \(s\).

The t-statistic is mostly used in the t-tests to test hypothesis of the form

$$H_0\::\:\beta=\beta_0$$

## The t-test

The t-test uses the sample mean as the t-statistic of choice, defined as

$$\langle X\rangle :=\frac{1}{n}\sum_i^n x_i$$

The sample standard deviation is calculated as

$$s = \left(\frac{1}{n-1}\sum_i\left(X_i - \langle X\rangle\right)^2\right)^{\frac{1}{2}}$$

If the population variance \(\sigma\) was known, it would have been possible to write under \(H_0\)

$$\langle X\rangle\sim\mathcal{N}(\mu,\:\sigma^2/n)\quad\Rightarrow\quad t=\frac{\langle X\rangle - \mu}{\sigma/\sqrt{n}}\sim\mathcal{N}(0,1)$$

and test against a normal distribution. However, the Slutsky's theorem proves that the distribution of the sample variance has little effect on the distribution of the t-test statistic.

> Proof:
> $$\begin{align*}&\sqrt{n}(\langle X\rangle - \mu)\:\xrightarrow[n\to\infty]{}\:\mathcal{N}(0,\sigma^2)&\text{(by the central limit theorem)}\\ &s^2\:\xrightarrow[n\to\infty]{}\:\sigma^2 &\text{(by the law of large numbers)}\\ &\frac{\sqrt{n}(\langle X\rangle - \mu)}{s}\:\xrightarrow[n\to\infty]{}\:\mathcal{N}(0,1) &\end{align*}$$

### One-sample t-test

The test compares the sample mean to the _known_ mean of the population \(\mu\). The hypothesis testing problem is

$$\begin{align*}&H_0\::\:x_i\:\:\stackrel{i.i.d.}{\sim}\:\:\mathcal{N}(\mu,\sigma^2)\:,\quad i=1,\ldots,n\\&H_a\::\:x_i\:\:\stackrel{i.i.d.}{\sim}\:\:\mathcal{N}(\tilde{\mu},\sigma^2)\:,\quad i=1,\ldots,n,\:\tilde{\mu}\end{align*}$$

The test statistic takes the form

$$t = \frac{\langle X\rangle - \mu}{s/\sqrt{n}}$$

with \(n\) sample size. The ratio \(s/\sqrt{n}\) is called _Standard error of the mean_.

The number of degrees of freedom is \(n-1\).

**Assumptions**

- \(\langle X\rangle\) follows the Gaussian distribution \(\mathcal{N}\left(\mu, \sigma^2/n\right)\). This is approximately true for large samples by the central limit theorem even when the sample is not normal.
- \((\langle X\rangle - \mu) \perp\!\!\!\perp \sigma\)
- \(s^2\left(n-1\right)/\sigma^2\), with \(\sigma\) variance of the population, follows a \(\chi^2\) distribution with \(n-1\) degrees of freedom. This is true if when the sample observations come from a Gaussian and are i.i.d.

The assumptions above are needed to guarantee _Exactness_, however, the sample does not need to be normal.
If the sample is not normal, the sample variance may deviate substantially from a \(\chi^2\).

### Two-samples t-test

The test compares the sample mean to the mean of a second sample to determine if they are likely drawn from the same population. The form of the test depends on how similar are the variances of the two samples.

The hypothesis testing problem is

$$\begin{align*}H_0\::\: &x_{i,1}\:\:\stackrel{i.i.d.}{\sim}\:\:\mathcal{N}(\mu,\sigma_1^2)\:,\quad i=1,\ldots,n_1\\&x_{i,2}\:\:\stackrel{i.i.d.}{\sim}\:\:\mathcal{N}(\mu,\sigma_2^2)\:,\quad i=1,\ldots,n_2\\H_a\::\: &x_{i,1}\:\:\stackrel{i.i.d.}{\sim}\:\:\mathcal{N}(\tilde{\mu},\sigma_1^2)\:,\quad i=1,\ldots,n_1,\:\tilde{\mu}\\&x_{i,2}\:\:\stackrel{i.i.d.}{\sim}\:\:\mathcal{N}(\tilde{\mu},\sigma_2^2)\:,\quad i=1,\ldots,n_2,\:\tilde{\mu}\end{align*}$$

#### Samples of equal size and variance

Applicable when \(n_1=n_2\) and \(s_1\sim s_2\). The test statistic is this case is

$$t = \frac{\langle X_1\rangle - \langle X_2\rangle}{\sqrt{\frac{s_1^2+s_2^2}{n}}}$$

The number of degrees of freedom is \(2n-2\).

**Assumptions**

- The two populations whould follow normal distributions.
- The original Student's t-test requires equal variances in the populations. The Welch's t-test is insensitive to differences in variances.
- The samples should be independent.

#### Samples with similar variances

Applicable when \(n_1\neq n_2\) and \(\frac{1}{2}\leq\frac{s_1}{s_2}\leq 2\). The test statistic is this case is

$$t = \frac{\langle X_1\rangle - \langle X_2\rangle}{\sqrt{\frac{\left(n_1 - 1\right)s_1^2 + \left(n_2 - 1\right)s_2^2}{n_1 + n_2 - 2}\left(\frac{1}{n_1} + \frac{1}{n_2}\right)}}$$

The number of degrees of freedom is \(n_1 + n_2 - 2\) and we have that

$$\langle X_1\rangle - \langle X_2\rangle\sim\mathcal{N}\left(0,\frac{\sigma^2}{n_1}+\frac{\sigma^2}{n_2}\right)$$

where \(\sigma\) is estimated by \(s\).

#### Different samples

Applicable when \(n_1\neq n_2\) and \(\frac{s_1}{s_2}<\frac{1}{2}\) or \(\frac{s_1}{s_2}>2\). This is also called the **Welch's t-test**. The test statistic is this case is

$$t = \frac{\langle X_1\rangle - \langle X_2\rangle}{\sqrt{\frac{s_1^2}{n_1}+\frac{s_2^2}{n_2}}}$$

The number of degrees of freedom \(df\) is

$$df=\frac{\left(\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}\right)^2}{\frac{\left(\frac{s_1^2}{n_1}\right)^2}{n_1 - 1} + \frac{\left(\frac{s_2^2}{n_2}\right)^2}{n_2 - 1}}$$

and we have that

$$\langle X_1\rangle - \langle X_2\rangle\sim\mathcal{N}\left(0,\frac{\sigma_1^2}{n_1}+\frac{\sigma_2^2}{n_2}\right)$$

where \(\sigma_\alpha\) is estimated by \(s_\alpha\).

### Paired t-test

The test compares the mean of the differences \(X_d\) between two samples of equal size \(n\) to \(\mu_0\). If the test must assess whether a difference exists between the two samples, \(\mu_0=0\). The statistic \(t\) is given by

$$t = \frac{\langle X_d\rangle - \mu_0}{s_d/\sqrt{n}}$$

Here, \(s_d\) is the standard deviation of \(X_d\).\
The number of degrees of freedom is \(n-1\).

## External links

- [t-Test on Wikipedia](https://en.wikipedia.org/wiki/Student%27s_t-test)
- [Welch's t-Test on Wikipedia](https://en.wikipedia.org/wiki/Welch%27s_t-test)
