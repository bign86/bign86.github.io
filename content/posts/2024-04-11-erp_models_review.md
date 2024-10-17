---
title: "Notes on *The Equity Risk Premium: A Review of Models*"
date: 2024-04-12
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
slug: notes-equity-risk-premium-review-models
---

The paper combines 20 Equity Risk Premium models used in literature and industry, and averages them to infer the ERP.

## Equity Risk Premium

The ERP is the compensation investors require to make them indifferent between holding the risky market portfolio or a risk-free asset. ERP incorporates expectations of future stock market returns.

The challange in estimating the ERP is that it is not clear what constitutes the market return and the risk-free rate. For the former, indeces are commonly used that do not represent the whole market and miss several components of wealh such as housing, private wealth, human capital.

The realized stock return $R_{t+k}$ between $t$ and $t+k$ is decomposed in

$$R_{t+k} = E_t[R_{t+k}] + \varepsilon_{t+k}$$

or an expected part given the information available at time $t$, and an error part.<br>
The ERP at time $t$ for the horizon $k$ is defined as

$$ERP_t(k) = E_t[R_{t+k}] - R^f_{t+k}$$

Three aspects:

* future returns and ERP are stochastic
* the ERP depends on the length of the future horizon $k$
* if investors are rational, $\varepsilon_{t+k}$ is othogonal to $E_t[R_{t+k}]$ and the ERP is smoother than realized excess returns

## Model types

### Historical

This type of model uses the historical mean of realized stock returns in excess of the contemporaneous risk-free rate to estimate the ERP. The model is mainly backward looking and requires defining the window of past result to use.

### Dividend Discount

The intuition is that dividends paid to shareholders are what give value to a stock. The price of the stock is thus

$$P_t = \frac{D_t}{\rho_t} + \sum_{i=1}\frac{E_t[D_{t+i}]}{\rho_{t+i}}$$

The discount factor depends on the ERP

$$\rho_{t+k} = 1 + R^f_{t+k} + ERP_t(k)$$

where the risk-free takes into account the time-value of money and the ERP the discounting associate with the riskiness of dividends.<br>An advantage of this type of model is that it is forward-looking.

### Cross-section regression

This method exploits the variation in returns across assets. The following regression is estimated

$$R^i_{t+k} - R^f_{t+k} = \alpha^i\cdot S_{t+k} + \beta^i\cdot F_{t+k} + I^i_{t+k}$$

with $S_{t+k}$ state variables such as inflation, unemployment, bond yield spread, $F_{t+k}$ systematic risk factors such as Fama-French factors, $I^i_{t+k}$ company-specific factors for asset $i$.

The regression above yields the exposures $\hat{\beta}$ that are employied in the following regression

$$R^i_{t+k} - R^f_{t+k} = \lambda_t(k)\cdot\hat{\beta}^i$$

that yields estimated coefficients $\hat{\lambda}_t(k)$. The product term on the right hand side of the equation above corresponding to the market proxy is the $ERP_t(k)$.

### Time-series regression

Time-series approaches are based on the relationships between stock returns and economic variables as in:

$$R_{t+k} - R^f_{t+k} = \alpha + \beta\cdot F_t + \varepsilon_t$$

with $F_t$ fundamental variables. The ERP is found from the estimated $\hat{\alpha}$ and $\hat{\beta}$:

$$ERP_t(k) = \hat{\alpha} + \hat{\beta}\cdot F_t$$

The advantages of this method are the simple implementation and the minimal assumptions required. Disadvantages are the need to choose the fundamental factors to include and that time-series ignore cross-sectional information. Note also that multifactor models often after poorer out-of-sample predictions than single-factor models.

### Surveys

Surveys are an easy altough time consuming way to collect a consensus from knowledgeable investors. Should be a good predictor of excess returns as stock prices are determined by the same market partecipants that are contributing to the survey.

## Ensemble determination of the ERP

The paper combines all the models (20) into a single determination of the ERP through a few steps of calculation

1. The single-model ERP estimates are de-meaned
2. The variance-covariance matrix is determined
3. <div>A principal component analysis (PCA) is carried out to determine the weight each model should have in the final determination. This is done throught he following equation:

    $$PC^{(1)}_t = \sum_m w_m \cdot ERP_{t,m}$$

    where $m$ runs on all models and $w$ are the weights. The contribution in terms of ERP for each model is calculated as

    $$ERP_{t,m} = \sum_i \lambda^{(i)}_m \cdot PC^{(i)}_t$$

    where $\lambda$ represents the weight of each model in the final ERP and $i$ runs on the principal components of the variance-covariance matrix.</div>

## Critique

The paper presents the results of the 20 models along with the result of the models ensemble. They analyse the heterogeinity of results they obtain from the models both as point-in-time results and as ERP term structure by changing the estimate horizon.

What is missing in the paper is a theoretical foundation to the idea of creating an ensemble of the underlying models nor a rationale for the choice of models to include. Without it there is no reason to believe that an average of the models should actually help converge the results towards an un-biased estimate.

It is interesting to see the comparison between models actually used in the industry but I wouldn't apply this averaging technique in real life.

## Link

[Paper PDF](https://www.newyorkfed.org/medialibrary/media/research/staff_reports/sr714.pdf)
