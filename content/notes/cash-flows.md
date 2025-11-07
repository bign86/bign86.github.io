---
title: Cash Flows
description: ""
date: 2025-08-16
preview: ""
draft: false
tags:
  - basic finance
categories:
  - finance
  - notes
math: true
slug: cash-flows
---

## Interest rates

An interest rate \(r\) is used to gross a present value \(PV\) to obtain the future value \(FV\) after a given period of time \(t\), known as the **tenor**. In most cases, the length of \(t\) is understood from the context.

$$FV = PV (1 + r)$$

Applying the same over \(n\) periods il called compounding. The result of compounding depends on how the interest rate is defined:

- **Simple interest rate**: The value grows by the same monetary quantity in every period
  $$PV(1 + nr)$$
- **Compound interest rate**: The value grows by the same percentage quantity in every period
  $$PV(1 + r)^{n}$$

_Annualization_: often interest rates are quoted based on a standard period (tipically one year, hence the word annualization) but may be compounded at a different frequency. In this case the convention is

$$PV \left(1 + \frac{r}{m}\right)^{nm}$$

For example a rate can be quoted as an annual value and be compounded monthly for 5 years. In this case \(n=5\) and \(m=12\). It is assumed the interest rate is given for a period of 1 year.

- **Continuous compounding**: The limit where the compounding frequency is infinite
  $$\lim_{m\rightarrow\infty} PV\left(1 + \frac{r}{m}\right)^{m} = PV\:e^{rm}$$

The continuous compounding is not used in everyday business, but has two important properties:

- It can be showed that the continuous compounding represents an upper limit of the periodic compounding.
- The exponential form is well behaved mathematically and helps keeping complex models in continuous time mathematically tractable. For example, the rate of growth of a sum of money would be
  - for the periodic compounding
    $$\frac{d}{dt}\:\left(1 + \frac{r}{m}\right)^{mt} = m\:\log\left(1 + \frac{r}{m}\right)\:\left(1 + \frac{r}{m}\right)^{mt}$$
  - for the continuous compounding
    $$\frac{d}{dt}\:e^{rt} = \:r\:e^{rt}$$

### Discount factors

If the future value of a present monetary amount is found by grossing, the opposite operation is called **discounting**. Assuming a single period of time of length \(t\):

$$PV = FV \frac{1}{(1 + r_t)} = FV\:d_t$$

where \(d_t\) is called _discount factor_ for the tenor \(t\).\
As interest rates are tend to be positive, discount factors tend to be lower than 1.

### Term structure

As the interest rate can be defined for any tenor \(t\), it is possible to create curves of interest rates at different tenors called the **term structure** or **yield curve**, where each point of the curve is called a **spot rate**. For example, assuming a constant yearly rate \(r_{1y}\), the 2-year interest rate can be defined as:

$$r_{2y} = \left(1 + r_{1y}\right)^2 - 1$$

and the interest rate at any other tenor could be found as

$$r_{ny} = \left(1 + r_{1y}\right)^n - 1$$

In practice, the interest rates follow a curve determined by complex relationships in the markets.

The term structure of spot rates univocally determines a **forward curve**. The points in the forward curve are the interest rates that can be used to gross a cash flow between two given tenors in the yield curve.\
Called \(s_1\) the spot rate applicable to gross a cash flow to 1 year from now, and \(s_2\) the spot rate applicable to gross a cash flow to 2 years from now, it must be verified that

$$\left(1 + s_2\right) = \left(1 + s_1\right) \left(1 + f_{1,2}\right)$$

where \(f_{1,2}\) is known as the **forward rate** between the 1-year and the 2-years tenors.

Similarly, the yield curve of spot rates also univocally determines a discount curve obtained as:

$$\frac{1}{1 + s_2} = d_2 = d_1\:d_{1,2} = \frac{1}{1 + s_1} \frac{1}{1 + f_{1,2}}$$

where \(d_{1,2}\) is known as a **forward discount factor**.
