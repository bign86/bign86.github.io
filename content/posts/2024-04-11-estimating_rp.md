---
title: Notes on *Estimating Equity Risk Premiums*
date: 2024-04-11
tags:
  - equity-risk-premium
  - investing
  - literature review
categories:
  - finance
author_profile: true
draft: false
comments: false
math: true
slug: notes-estimating-equity-risk-premiums
---

_Paper_: "Estimating Equity Risk Premiums", A. Damodaran, Stern School of Business

The paper reviews the historical approach to the calculation of the equity risk premium.

## Risk and return

There are several competing risk & return models but all agree on some common view of risk:

* risk is defined in terms of returns variance
* risk is measured from the point of view of the well diversified marginal investor
* hence, only non-diversifiable risk should be rewarded

Risk therefore commonly broken into a *firm-specific* component and a *market* component, the latter being rewarded.

Common approaches to measuring risk:

* *Capital Asset Pricing Model (CAMP)*: beta measured against a market portfolio
* *Arbitrage Pricing Model (APM)*: beta measured against multiple market risk factors
* *Multi-Factor Models*: beta is measured against macro economic factors

In all cases the general formula for the expected return is

$$E[R] = R_f + \sum_i \beta_i R^P_i$$

with \(R_f\) risk-free rate, \(\beta_i\) exposure to the factor \(i\), and \(R^P_i\) risk premium for factor \(i\). These are also the required inputs.

|              | What we **want** to measure | What we **do** measure |
|:------------:| ------------------------------------------------------- | ------ |
| \(\beta_i\)  | as how much market risk there is in any investment added to a fully (within and across asset classes) diversified portfolio | as how much premium an investors requires over the risk-free to be compensated for the risk |
| \(R^P_i\)    | as how much market risk there is compared to a proxy for a local market | as an historical measure of premia earned by stocks over default free securities |

## Historical approach

The historical approach to the risk premium involves taking time series of actual stock returns compared to actual default-free (government) securities returns. The difference between the two computed annually gives the risk premium. This simple approach yields a wild variaty of results. This is due to:

* *Time period used*: the length if the time series is a free choice. Shorter periods
  * [pro] investor's risk aversion is likely to change over time, shorter periods are consistent with current behavior
  * [pro] gives a more updated estimate
  * [con] very large noise, standard errors may be higher than the actual premium

  Note that the longer the time horizons, the higher is the effect of survivorship bias on the estimates.
* *Risk-free*: the proxy used for the risk-free asset is usually a government security. Due to the yield curve, different tenor for the proxy lead to different risk premia. In general, the tenor used to calculate the risk-free rate and the tenor used to calculate the premium must be consistent.
* *Arithmetic vs geometric average*: the arithmetic average is correct if
  * annual returns are uncorrelated
  * projections are made for the next year only
  
  Usually neither of these assumptions is true. Therefore, geometric average is preferable.

### Emerging markets

Estimating risk premia is difficult in developed markets some of which, like the European ones, are mature but dominated by few large companies and where many businesses are private. In emerging markets the situation is made worse by the very short history, high volatility, low valumes.

The calculation of risk premia for emerging markets \(R^{PE}\) could follow this approach:

$$R^{PE} = R^{mat} + R^{cty}$$

with \(R^{mat}\) base risk premium from a mature market and \(R^{cty}\) country premium for the specific emerging market.

The country premium would be diversifiable, and therefore should be neglected, if the equity markets across countries were uncorrelated. However, there is ample evidence this is not the case. The evaluation of the country premium comprises the following steps:

1. *Measure country default risk*\
    One popular option is to employ the ratings assigned to countries' debt by rating agencies to have the country default risk \(D^{cty}\).
   * [pro] easily accessible
   * [pro] many factors affecting credit risk also drive equity risk
   * [con] rating agencies lag markets
   * [con] the focus on default risk may neglect other risks relevant for risk premia

2. *Get country equity risk*\
      We expect country equity risk \(R^{cty}\) to be larger than the default risk. The ratio between the equity market and bond market volatility can be used to rescale the default risk
      $$R^{cty} = D^{cty}\frac{\sigma_e}{\sigma_b}$$
      with \(\sigma_e\) equity market volatility and \(\sigma_b\) bond market volatility. Note that the calculation of volatilities requires a time horizon that should be consistent with the rest of the calculation.

3. *Estimate individual asset exposure to country risk premium*\
      There are a few viable approach:
   * Assume all stocks are equally exposed to country risk
        $$E[R] = R_f^{mat} + \beta\cdot R^{mat} + R^{cty}$$</div>
   * Assume exposure is proportional to exposure to market risk
        $$E[R] = R_f^{mat} + \beta\cdot (R^{mat} + R^{cty})$$</div>
   * Estimate an exposure \(\lambda\) to country risk
        $$E[R] = R_f^{mat} + \beta\cdot R^{mat} + \lambda\cdot R^{cty}$$
        The exposure to country risk is proportional to the amount of revenues the company makes in the country. In general, if the company has interests in several countries, an exposure should be calculated for each one.

## Implied equity premia

Equity premia may be estimated using discounted cash flow methodologies, without the need of historical data. The advantage of the DCF methodologies, is that they are market-driven and current, as they rely on the latest available data, not historical ones.

Implied premia show differences compared to historical ones that can be summarized as follows:

* Implied equity risk premia are more volatile than historical risk premia
* Implied equity risk premia are generally lower than historical risk premia
* Implied equity risk premia correlate with expected inflation and interest rate changes

## Link

[Paper PDF](https://pages.stern.nyu.edu/~adamodar/pdfiles/papers/riskprem.pdf)
