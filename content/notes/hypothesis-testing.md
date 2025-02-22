---
title: Hypothesis testing
description: ""
date: 2024-12-28
preview: ""
draft: false
tags: [statistics]
categories: [math, notes]
math: true
---

_Statistical hypothesis testing_ is a systematic procedure used in statistics to test whether a particular hypotesis on a population is supported by the available sample data.
In a nutshell, hypothesis testing calculates a statistic of choice on the sample data, and assesses how likely it is that the population would show a similarly extreme value. Small deviations between sample and population statistic are likely to be due to sampling error, but large deviations may indicate the sample does not belong to the population.

In a stardard setting, a research hypothesis to be evaluated is called the alternate hypothesis. Its opposite is called the null hypothesis and is the relation that is tested. The outcome of the test determines whether the null hypothesis can be rejected thus accepting the alternate, or not.

## General procedure

The folllowing elements are needed for hypothesis testing:

- _Null hypothesis_ \(H_0\): an unproven statement tested through the hypothesis test. The aim is to disprove the null hypothesis by showing that the available data support its rejection.
- _Alternative hypothesis_ \(H_a\): alternative statement that is going to be accepted in case \(H_0\) is rejected. Usually, the desired result.
- _Sample data_: observed sample of data used to test the null hypothesis. The sample must be a good proxy of the population for the test to be meaningful.
- _Sample distribution_: determine which probability distribution to use for the test.
- _Test statistic_: metric used on the data to evaluate the null hypothesis.
- _p-value_ \(p\): probability of observing results at least as extreme as the ones actually observed assuming the null hypothesis is true.
- _Significance level_ \(\alpha\): probability of (wrongfully) rejecting a true null hypothesis. It is used as the confidence threshold on the p-value giving sufficient comfort on the rejection of the null hypothesis. Typical values are 5\% and 1\%.

These are the steps to test a hypothesis:

1. Formulate the research hypothesis \(H_a\) and the joint null hypothesis \(H_0\)
2. Determine the best distribution to describe the phenomenon under study
3. Choose a test statistic on the distribution
4. Choose the minimum significance level to reject the null hypothesis.
5. Calculate the test statistic on the sample data, along with the confidence level (p-value) and compare the latter with the significance level \(\alpha\).
6. Draw the conclusions on the test. If \(p\leq\alpha\) there is sufficient evidence to reject the \(H_0\) and accept \(H_a\). In this case, it is said the result is _statistically significant_. If the data are not supportive of the rejection, the null hypothesis cannot be rejected and the alternate is discarded.

### Types of tests

Many statistical tests have been defined over time. Regardless of the specific test of choice, it is important that the sampling distribution under the null hypothesis can be calculated even if only approximated. Without a sampling distribution it is not possible to calculated the p-value and no conclusions can be drawn on the test.

A test can be one- or two-tailed, depending on whether they consider only one or both sides of the sampling distribution. The choice between the two variants depends entirely on the research question to answer.

- One-tailed tests only compare the test statistic with one threshold in either the right or left tail of the distribution.
- Two-tailed tests allow the test statistic to be compared with both tails. Hence, two-tailed tests do not assume a direction of the test statistic.

Tests can also differ for the sample data used:

- One-sample tests compare the sample data to a population whose characteristics are known.
- Two-samples tests compare two samples to assess how similar they are.
- Paired tests are used as an alternative to two-samples tests. Instead of directly comparing the two samples, the difference between the two samples becomes the data set to be tested against zero.

## p-value and statistical significance

Given a test statistic \(t\) under the distribution \(P\), the p-value \(p\) is defined as:

$$p := \begin{cases}\text{Pr}\left(P\geq t\:\vert\: H_0\right) & \text{one sided, right-tail}\\\text{Pr}\left(P\leq t\:\vert\: H_0\right) & \text{one sided, left-tail}\\2\:\text{min}\left\{\text{Pr}\left(P\geq t\:\vert\: H_0\right)\:,\:\text{Pr}\left(P\leq t\:\vert\: H_0\right)\right\} & \text{two sided}\end{cases}$$

Therefore the p-value is the probability under \(H_0\) of observing a test statistic at least as extreme as the observed one. In other words, the p-value measures the strenght of the evidence agaist \(H_0\).

Statistical significance and p-value are very important and often misused concepts in hypothesis testing. It has even been recently proposed to stop using them. Below are the most common misconceptions/pitfalls.

**Meaning of statistical significance**:

- A large p-value means that evidence against \(H_0\) is weak, however, it _does NOT_ mean that there is a strong evidence in favour of \(H_a\).
- Failure to reject \(H_0\) _does NOT_ mean the null hypothesis _must_ be true. It means that the observed data sample does not provide enough comfort to reject it.
- Similarly, a rejection _does NOT_ mean that \(H_0\) _must_ be false, but only that the data support its rejection up to a confidence level.
- The p-value should _NOT_ be thought as \(P(H_0\:\vert\:\text{sample data})\).

**Effect size**:\
A statistically significant result _does not_ inform on the _magnitude_ of the effect, i.e. an effect can be statistically significant but minuscule and irrelevant for the phenomenon under study. Effect size measures are used to quantify the strenght of an effect and should be reported alongside statistical significance.

**False positives**:\
A statistically significant result could be a false positive (with a probability determined by \(\alpha\)). Care is needed to make sure the possibility of false positives is minimized. One way is to attempt at independently reproducing the results of the study. Unreproducible results may be due to false positives.

## Errors

Hypothesis testing recognizes two types of errors.

- _Type I error_: error made when a true null hypothesis is (wrongfully) rejected and the false alternate hypothesis accepted. It corresponds to the significance level \(\alpha\).
- _Type II error_: error made when a false null hypothesis is (wrongfully) accepted.

These errors can be minimized by a carefull design of the test, however, they cannot be eliminated. Moreover, there is a trade-off between errors so that minimizing the chance of committing one, it will increase the probability of committing the other.

## Common tests

- [Student's t-test](/notes/student-t-test)
- [Tests on variance](/notes/variance-tests)
