---
title: Tests on variance
description: ""
date: 2025-01-28
preview: ""
draft: false
tags: [statistics]
categories: [math, notes]
math: true
---

This is a collection of methods that can be used to test whether samples are drawn from populations of equal variance.

## Chi-square test

Given a a sample \(X\) of size \(n\), the Chi-square test tests whether the sample has variance \(s\) equal to a specified value \(\sigma\).

$$\begin{align*}&H_0\::\:s^2=\sigma^2\\&H_a\::\:s^2 < \sigma^2\:,\quad \text{left-tail test}\\&\qquad\:\: s^2 > \sigma^2\:,\quad \text{right-tail test}\\&\qquad\:\: s^2 \neq \sigma^2\:,\quad \text{two-tailed test}\end{align*}$$

\(s\) represents the sample variance defined for a generic random variable \(\xi\) of size \(l\) as:

$$s_\xi^2 := \frac{1}{l-1}\sum_i^l\left(\xi_i - \langle\xi\rangle\right)$$

The test statistic is given by:

$$T=(n-1)\frac{s^2}{\sigma^2}$$

The test statistic follows a Chi-squared distribution \(\chi^2(d)\) with degree of freedom

$$d_1 = n - 1$$

The \(H_0\) is rejected at the significance level \(\alpha\) if:

- Left tail test: \(T < \chi^2(\alpha;\:d)\).
- Right tail test: \(T > \chi^2(1-\alpha;\:d)\).
- Two-tailed test: \(T < \chi^2(\frac{\alpha}{2};d)\) or \(T > \chi^2(1-\frac{\alpha}{2};\:d)\).

## F-test

Given the two samples \(X\) of size \(n\) and \(Y\) of size \(m\), the F-test tests the null hypothesis that two normal populations have equal variance:

$$\begin{align*}&H_0\::\:s_X^2=s_Y^2\\&H_a\::\:s_X^2 < s_Y^2\:,\quad \text{left-tail test}\\&\qquad\:\: s_X^2 > s_Y^2\:,\quad \text{right-tail test}\\&\qquad\:\: s_X^2 \neq s_Y^2\:,\quad \text{two-tailed test}\end{align*}$$

The test statistic is given by:

$$F=\frac{s_X^2}{s_Y^2}$$

The test statistic follows a Fisher-Snedecor distribution \(\mathcal{F}(d_1,d_2)\) with degrees of freedom

$$d_1 = n - 1\qquad d_2 = m - 1$$

The \(H_0\) is rejected at the significance level \(\alpha\) if:

- Left tail test: \(F < \mathcal{F}(\alpha;\:d_1,d_2)\).
- Right tail test: \(F > \mathcal{F}(1-\alpha;\:d_1,d_2)\).
- Two-tailed test: \(F < \mathcal{F}(\frac{\alpha}{2};d_1,d_2)\) or \(F > \mathcal{F}(1-\frac{\alpha}{2};\:d_1,d_2)\).

**Assumptions**

- \(X\) and \(Y\) are independent and identically distributed (\(i.i.d.\)).
- \(X\) and \(Y\) are normal: this assumptions is extremely important as the test is particularly sensitive to non-normality. Great care is needed to check the assumption holds before using this test. In case of non-normal sample, the test could transform in a non-normality test.

## Bartlett's test

The Bartlett's test[^2] is applied to a group of \(k\) samples \(X_1,\ldots,X_k\) of unequal sizes \(n_1,\ldots,n_k\):

$$\begin{align*}&H_0\::\:s_1^2=s_2^2=\ldots =s_k^2\\&H_a\::\:s_i^2 \neq s_j^2\:,\quad \text{for at least one pair }(i,\:j)\end{align*}$$

The test statistic is given by:

$$T=\frac{\left(N - k\right)\ln s_p^2 - \sum_i^k\left(n_i - 1\right)\ln s_i^2}{1 + \frac{1}{3\left(k-1\right)}\left(\sum_i^k\frac{1}{n_i - 1} - \frac{1}{N - k}\right)}$$

with \(N=\sum_i^kn_i\) and \(s_p^2\) the pooled variance defined as

$$s_p^2 := \frac{1}{N-k}\sum_i^k s_i^2\left(n_i-1\right)$$

The test statistic follows a Chi-squared distribution \(\chi^2(d)\) with degree of freedom

$$d = k - 1$$

The \(H_0\) is rejected at the significance level \(\alpha\) if:

$$T > \chi^2(1-\alpha;\:k-1)$$

**Assumptions**

- The samples are normal: the test is particularly sensitive to non-normality, similarly to the F-test.

## Levene's test

Levene's test is similar to the Bartlett's test as it compares the variances of a group of \(k\) samples under the null hypothesis the variances are equal. However, is less sensitive to the normality of the data, and performs better in cases where this assumption does not strictly hold.

$$\begin{align*}&H_0\::\:s_1^2=s_2^2=\ldots =s_k^2\\&H_a\::\:s_i^2 \neq s_j^2\:,\quad \text{for at least one pair }(i,\:j)\end{align*}$$

The test statistic is given by:

$$W=\frac{N - k}{k - 1}\frac{\sum_i^k n_i\left(\mu(x_{ij};\:j) - \mu(x_{ij};\:i,j)\right)^2}{\sum_i^k\sum_j^{n_i} \left(x_{ij} - \mu(x_{ij};\:j)\right)^2}$$

where \(x_{ij}\) are the samples data and \(\mu(x;\:\eta)\) represents a central tendency function applied on the data \(x\) over the set of indices \(\eta\).

The test statistic follows a Fisher-Snedecor distribution \(\mathcal{F}(d_1,d_2)\) with degrees of freedom

$$d_1 = k - 1\qquad d_2 = N - k$$

with \(N\) defined as in the Bartlett's test.

The \(H_0\) is rejected at the significance level \(\alpha\) if:

$$W > \mathcal{F}(\alpha;\:d_1,d_2)$$

### Flavors of Levene's test

Three measures of central tendency \(\mu\) have been reported in literature:

- \(\vert\text{mean}\vert\): the absolute mean was used in the original Levene's paper[^1].
- \(\vert\text{median}\vert\): the absolute median has been proposed by Brown and Forsythe[^3] and is also known as _Brown-Forsythe test_. It proved best for Chi-square distributed data.
- \(\vert\text{trimmed mean}\vert\): the absolute trimmed mean has been proposed by Brown and Forsythe[^3] and it proved best with fat-tailed data.

[^1]: Levene, H. (1960), _In Contributions to Probability and Statistics: Essays in Honor of Harold Hotelling_, Stanford University Press, pp. 278-292.

[^2]: Snedecor, George W. and Cochran, William G. (1989), _Statistical Methods_, Eighth Edition, Iowa State University Press.

[^3]: Brown, M. B. and Forsythe, A. B. (1974), Journal of the American Statistical Association, 69, pp. 364-367.
