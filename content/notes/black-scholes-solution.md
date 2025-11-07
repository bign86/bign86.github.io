---
title: Solving the Black-Scholes equation
description: ""
date: 2025-09-16
preview: ""
draft: false
tags:
  - basic finance
  - options
  - Black-Scholes
categories:
  - finance
  - notes
math: true
slug: black-scholes-solution
---

The Black-Scholes equation derivation has been presented [here](/notes/black-scholes). The final equation rewritten in compressed form is

$$\partial_t V + \frac{1}{2}\sigma^2 S^2\:\partial^2_{S^2} V + r S\:\partial_S V - r V = 0\tag{1}$$

The equation is equivalent to a _Heat equation_ from physics, a problem with a well known solution. The solution of Black-Scholes proceeds through variable substitutions with the aim of transforming equation (1) in the form of a Heat equation.

### The Heat equation

The Heat equation has the following form

$$\partial_t f = \Delta f\qquad\text{in 1D:}\quad \partial_t f = \partial^2_{x^2} f$$

Therefore, we want to transform the Black-Scholes equation into an equation where the coefficients of the time derivative and the second derivative of price is equal to 1, while all other coefficients are zero. To obtain this, we recast the equation several times through variable substitutions until the correct form is obtained. The Heat equation has a well know solutions. The last step is to unwind the variable substitutions to recast the solution in terms of the quantitites in the Black-Scholes equation.

#### First substitution

The first step achieves a coefficient equal to 1 for the time derivative and the second derivative in the price. Set

$$\begin{cases}
\tau := \frac{1}{2}\sigma^2(T-t)\\
S := K\:e^x\quad\Rightarrow\quad x := \ln\left(\frac{S}{K}\right)
\end{cases}\qquad\Longrightarrow\qquad V(S, t) = K\:v(x, \tau)  \tag{2}$$

It is useful to calculate

$$\partial_t \tau = -\frac{1}{2}\sigma^2,\qquad\partial_S x = \partial_S\ln\left(\frac{S}{K}\right) = \frac{1}{S} \tag{3}$$

Using the results in (3), calculate the derivatives from equation (1):

$$\begin{align*}
\frac{\partial V}{\partial t} &= K\:\frac{\partial v}{\partial t} = K\:\frac{\partial v}{\partial \tau}\frac{\partial \tau}{\partial t} = K\:\frac{\partial v}{\partial \tau}\left(-\frac{1}{2}\sigma^2\right)\\
\frac{\partial V}{\partial S} &= K\:\frac{\partial v}{\partial S} = K\:\frac{\partial v}{\partial x}\frac{\partial x}{\partial S} = K\:\frac{\partial v}{\partial x}\frac{1}{S}\\
\frac{\partial^2 V}{\partial S^2} &= \frac{\partial}{\partial S}\left(\frac{\partial V}{\partial S}\right) = \frac{\partial}{\partial S}\left(\frac{K}{S}\frac{\partial v}{\partial x}\right) \\
&= -\frac{K}{S^2}\frac{\partial v}{\partial x} + \frac{K}{S}\frac{\partial}{\partial S}\frac{\partial x}{\partial x}\frac{\partial v}{\partial x}\\
&= -\frac{K}{S^2}\frac{\partial v}{\partial x} + \frac{K}{S}\left(\frac{\partial}{\partial x}\frac{\partial v}{\partial x}\right)\left(\frac{\partial x}{\partial S}\right) & \text{not pretty but effective}\\
&= -\frac{K}{S^2}\frac{\partial v}{\partial x} + \frac{K}{S}\frac{\partial^2 v}{\partial x^2}\frac{1}{S}\\
&= -\frac{K}{S^2}\frac{\partial v}{\partial x} + \frac{K}{S^2}\frac{\partial^2 v}{\partial x^2}\\
&= \frac{K}{S^2}\left(\frac{\partial^2 v}{\partial x^2} - \frac{\partial v}{\partial x}\right)\\
\end{align*}$$

Substituting the derivatives just found and equation (2) we recast equation (1) in terms of \(\tau\), \(x\), and \(v\):

$$\begin{align*}
&\partial_t V + \frac{1}{2}\sigma^2 S^2\:\partial^2_{S^2} V + r S\:\partial_S V - r V = 0\\
&\left(-\frac{1}{2}\sigma^2 K\right)\partial_\tau v + \frac{1}{2}\sigma^2 S^2\:\frac{K}{S^2}\left(\partial^2_{x^2} v - \partial_x v\right) + r SK\partial_x v\frac{1}{S} - r V = 0 & \text{subst. derivatives}\\
&\left(-\frac{1}{2}\sigma^2 K\right)\partial_\tau v + \frac{1}{2}\sigma^2 S^2\:\frac{K}{S^2}\left(\partial^2_{x^2} v - \partial_x v\right) + r SK\partial_x v\frac{1}{S} - r Kv = 0 & \text{subst. }V\\
&\left(-\frac{1}{2}\sigma^2\right)\partial_\tau v + \frac{1}{2}\sigma^2 S^2\:\frac{1}{S^2}\left(\partial^2_{x^2} v - \partial_x v\right) + r S\partial_x v\frac{1}{S} - r v = 0 & \text{dividing by }K\\
&\left(-\frac{1}{2}\sigma^2\right)\partial_\tau v + \frac{1}{2}\sigma^2 \left(\partial^2_{x^2} v - \partial_x v\right) + r\partial_x v - r v = 0 & \text{simplifying}\\
&-\partial_\tau v + \partial^2_{x^2} v - \partial_x v + \frac{r}{\frac{1}{2}\sigma^2}\partial_x v - \frac{r}{\frac{1}{2}\sigma^2}v = 0 & \text{dividing by }\frac{1}{2}\sigma^2\\
&-\partial_\tau v + \partial^2_{x^2} v - \partial_x v + \mu\partial_x v - \mu v = 0 & \text{subst. }\mu=\frac{r}{\frac{1}{2}\sigma^2}\\
\end{align*}$$

The final equation reads

$$-\partial_\tau v + \partial^2_{x^2} v + \left(\mu - 1\right)\partial_x v - \mu v = 0 \tag{4}$$

with

$$\mu=\frac{r}{\frac{1}{2}\sigma^2}\:,\qquad x = \ln\left(\frac{S}{K}\right)\:,\qquad\tau = \frac{1}{2}\sigma^2(T-t)\:,\qquad v = \frac{V}{K}$$

The original equation (1) depended on the 4 dimensioned parameters \(K\), \(T\), \(\sigma^2\), and \(r\). The recasted equation (4) depends on only 2 parameters, \(\mu\) and \(\tau\), both dimensionless.

#### Second substitution

Compared to the Heat equation, equation (4) has two additional terms that must be eliminated. To do so, another substitution is used. Take

$$v(x,\tau) := e^{\alpha x + \beta\tau}\:u(x,\tau) \tag{5}$$

As before, we start calculating the derivatives found in (4):

$$\begin{align*}
\frac{\partial v}{\partial \tau} &= \beta\:e^{\alpha x + \beta\tau}\:u + e^{\alpha x + \beta\tau}\frac{\partial u}{\partial \tau}\\
\frac{\partial v}{\partial x} &= \alpha\:e^{\alpha x + \beta\tau}\:u + e^{\alpha x + \beta\tau}\frac{\partial u}{\partial x}\\
\frac{\partial^2 v}{\partial x^2} &= \alpha^2\:e^{\alpha x + \beta\tau}\:u + 2\alpha\:e^{\alpha x + \beta\tau}\frac{\partial u}{\partial x} + e^{\alpha x + \beta\tau}\frac{\partial^2 u}{\partial x^2}\\
\end{align*}$$

Subtituting the derivatives in (4), and setting \(e^{\alpha x + \beta\tau} = \varepsilon\) to simplify the notation, we obtain:

$$\begin{align*}
&-\partial_\tau v + \partial^2_{x^2} v + \left(\mu - 1\right)\partial_x v - \mu v = 0\\
&-\beta\varepsilon\:u - \varepsilon\partial_\tau u + \alpha^2\varepsilon\:u + 2\alpha\varepsilon\partial_x u + \varepsilon\partial^2_{x^2} u + \left(\mu - 1\right)\left(\alpha\varepsilon\:u + \varepsilon\partial_x u\right) - \mu v = 0 & \text{subst. derivatives}\\
&-\beta\varepsilon\:u - \varepsilon\partial_\tau u + \alpha^2\varepsilon\:u + 2\alpha\varepsilon\partial_x u + \varepsilon\partial^2_{x^2} u + \left(\mu - 1\right)\left(\alpha\varepsilon\:u + \varepsilon\partial_x u\right) - \mu \varepsilon\:u = 0 & \text{subst. }v\\
&-\beta\:u - \partial_\tau u + \alpha^2\:u + 2\alpha\partial_x u + \partial^2_{x^2} u + \left(\mu - 1\right)\left(\alpha\:u + \partial_x u\right) - \mu \:u = 0 & \text{dividing by }\varepsilon\\
&-\partial_\tau u + \left[\alpha\left(\mu - 1\right) -\beta + \alpha^2 - \mu\right]u + \left[2\alpha + \left(\mu - 1\right)\right]\partial_x u + \partial^2_{x^2} u = 0 & \text{rearranging} \tag{6}\\
\end{align*}$$

To obtain a Heat equation the coefficients of \(u\) and \(\partial_x u\) must be zero. Therefore, to remove the \(\partial_x u\) coefficient

$$2\alpha + \left(\mu - 1\right) = 0\quad\Rightarrow\quad \alpha := -\frac{1}{2}\left(\mu - 1\right)$$

and then to remove the \(u\) coefficient set

$$\begin{align*}
\alpha\left(\mu - 1\right) - \beta + \alpha^2 - \mu &= 0\\
\beta &= \alpha^2 + \alpha\left(\mu - 1\right) - \mu\\
&= \frac{1}{4}\left(\mu - 1\right)^2 + - \frac{1}{2}\left(\mu - 1\right)^2 - \mu\\
&= - \frac{1}{4}\mu^2 - \frac{1}{2}\mu - \frac{1}{4}\\
&= - \frac{1}{4}\left(\mu^2 + 2\mu + 1\right)\\
\Rightarrow\quad \beta &:= -\frac{1}{4}\left(\mu + 1\right)^2\\
\end{align*}$$

Substituting in (6) and rearranging, we obtain

$$\begin{align*}
&-\partial_\tau u + \left[\alpha\left(\mu - 1\right) - \beta + \alpha^2 - \mu\right]u + \left[2\alpha + \left(\mu - 1\right)\right]\partial_x u + \partial^2_{x^2} u = 0\\
&-\partial_\tau u + \left[-\frac{1}{2}\left(\mu - 1\right)^2 - \beta + \frac{1}{4}\left(\mu - 1\right)^2 - \mu\right]u + \left[-\left(\mu - 1\right) + \left(\mu - 1\right)\right]\partial_x u + \partial^2_{x^2} u = 0 & \text{subst. }\alpha\\
&-\partial_\tau u + \left[-\frac{1}{4}\left(\mu - 1\right)^2 - \beta - \mu\right]u + \partial^2_{x^2} u = 0\\
&-\partial_\tau u + \left[-\frac{1}{4}\left(\mu - 1\right)^2 + \frac{1}{4}\left(\mu + 1\right)^2 - \mu\right]u + \partial^2_{x^2} u = 0 & \text{subst. }\beta\\
&-\partial_\tau u + \partial^2_{x^2} u = 0\\
\end{align*}$$

Finally, the Heat equation is found

$$\partial_\tau u = \partial^2_{x^2} u \tag{7}$$

#### Initial condition

The initial condition of the differential equation on \(V(S_t, 0)\) must be transformed accordingly into a condition on \(u(x, 0)\). Starting from equation (5) and applying the transformations we get

$$\begin{align*}
u(x,\tau) &= e^{-\alpha x - \beta\tau}\:v(x,\tau)\\
u(x,0) &= e^{-\alpha x}\:v(x,0) & \text{with }\tau=0\\
&= e^{\frac{1}{2}\left(\mu - 1\right) x}\:v(x,0) & \text{subst. }\alpha\\
&= e^{\frac{1}{2}\left(\mu - 1\right) x}\frac{V(S,0)}{K} & \text{subst. }v(x,0)\\
\end{align*}$$

The value of the option must be recast as well in terms of the rescaled price \(x\). Therefore,

$$\begin{align*}
\frac{V}{K} &= \frac{1}{K}\max\left[S - K, 0\right]\\
&= \frac{1}{K}\max\left[Ke^x - K, 0\right] & \text{subst. }S\\
&= \max\left[e^x - 1, 0\right] \tag{8}\\
\end{align*}$$

Substituting (8) into the expression of the initial condition

$$\begin{align*}
u(x,0) &= e^{\frac{1}{2}\left(\mu - 1\right) x}\frac{V(S,0)}{K}\\
&= e^{\frac{1}{2}\left(\mu - 1\right) x}\max\left[e^x - 1, 0\right] & \text{subst. (8)}\\
&= \max\left[e^{\frac{1}{2}\left(\mu + 1\right) x} - e^{\frac{1}{2}\left(\mu - 1\right) x}, 0\right] \tag{9}\\
\end{align*}$$

### The solution

The solution to the Heat equation is the well know result

$$u(x,\tau) = \frac{1}{2\sqrt{\pi\tau}}\int_{-\infty}^{\infty} u_0(s)\:e^{-\frac{\left(x - s\right)^2}{4\tau}}\:ds \tag{10}$$

where \(u_0(s)\) is the initial condition in equation (9). This is the solution to the Black-Scholes equation. However, the solution in this form depends on the rescaled quantities and is not practical to use in the real world. To make use of it, all the variable changes done so far must be unwound.\
The first goal is to transform the exponent into the standard form of a Normal distribution \(-y^2/2\). Define

$$z := \frac{(s - x)}{\sqrt{2\tau}}\qquad\Longrightarrow\qquad \begin{cases}dz = -\frac{1}{\sqrt{2\tau}}dx\\ds = dx\end{cases}$$

and substitute into (10)

$$\begin{align*}
u(x,\tau) &= \frac{1}{\sqrt{2\pi}}\int_{-\infty}^{\infty} u_0(x + z\sqrt{2\tau})\:e^{-\frac{z^2}{2}}\:dz\\
&= \frac{1}{\sqrt{2\pi}}\int_{-\infty}^{\infty} \max\left[e^{\frac{1}{2}\left(\mu + 1\right) (x + z\sqrt{2\tau})} - e^{\frac{1}{2}\left(\mu - 1\right) (x + z\sqrt{2\tau})}, 0\right]\:e^{-\frac{z^2}{2}}\:dz & \text{subst. (9)}\\
&= \frac{1}{\sqrt{2\pi}}\int_{-\frac{x}{\sqrt{2\tau}}}^{\infty} \left[e^{\frac{1}{2}\left(\mu + 1\right) (x + z\sqrt{2\tau})} - e^{\frac{1}{2}\left(\mu - 1\right) (x + z\sqrt{2\tau})}\right]\:e^{-\frac{z^2}{2}}\:dz & \text{see (*)}\\
&= \frac{1}{\sqrt{2\pi}}\int_{-\frac{x}{\sqrt{2\tau}}}^{\infty} e^{\frac{1}{2}\left(\mu + 1\right) (x + z\sqrt{2\tau})}\:e^{-\frac{z^2}{2}}\:dz - \frac{1}{\sqrt{2\pi}}\int_{-\frac{x}{\sqrt{2\tau}}}^{\infty} e^{\frac{1}{2}\left(\mu - 1\right) (x + z\sqrt{2\tau})}\:e^{-\frac{z^2}{2}}\:dz\\
&= \frac{1}{\sqrt{2\pi}}\int_{-\frac{x}{\sqrt{2\tau}}}^{\infty} e^{\frac{1}{2}\left(\mu + 1\right) (x + z\sqrt{2\tau})-\frac{z^2}{2}}\:dz - \frac{1}{\sqrt{2\pi}}\int_{-\frac{x}{\sqrt{2\tau}}}^{\infty} e^{\frac{1}{2}\left(\mu - 1\right) (x + z\sqrt{2\tau})-\frac{z^2}{2}}\:dz \tag{11}\\
\end{align*}$$

(*): The presence of the max function allows to change the integration limits to where the function \(u\) is positive.

$$u_0(x + z\sqrt{2\tau}) = e^{\frac{1}{2}\left(\mu + 1\right) (x + z\sqrt{2\tau})} - e^{\frac{1}{2}\left(\mu - 1\right) (x + z\sqrt{2\tau})} > 0$$
$$\text{true when }\quad x + z\sqrt{2\tau} > 0\qquad\Longrightarrow\qquad z > -\frac{x}{\sqrt{2\tau}}$$

The goal is still to write the integrals in the form of a Normal distribution. Hence, we need splitting the exponents in terms depending on \(z\) and costant terms that can go out of the integral. Therefore, for the exponent in the first integral

$$\begin{align*}
\frac{1}{2}\left(\mu + 1\right) (x + z\sqrt{2\tau})-\frac{z^2}{2} &= \frac{1}{2}\left(\left(\mu + 1\right) z\sqrt{2\tau} - z^2\right) + \frac{1}{2}\left(\mu + 1\right) x\\
&=  \frac{1}{2}\left(\left(\mu + 1\right) z\sqrt{2\tau} - z^2\right) + \frac{1}{2}\left(\mu + 1\right) x + \frac{1}{4}\tau(\mu+1)^2 - \frac{1}{4}\tau(\mu+1)^2\\
&= -\frac{1}{2}\left(-\left(\mu + 1\right) z\sqrt{2\tau} + z^2  + \frac{1}{2}\tau(\mu+1)^2\right) + \frac{1}{2}\left(\mu + 1\right) x + \frac{1}{4}\tau(\mu+1)^2\\
&= -\frac{1}{2}\left(z - \sqrt{\frac{\tau}{2}}(\mu+1)\right)^2 + \frac{1}{2}\left(\mu + 1\right) x + \frac{1}{4}\tau(\mu+1)^2\\
\end{align*}$$

Applying this to the first integral and extracting the constant factor yields

$$\begin{align*}
&\frac{1}{\sqrt{2\pi}}\int_{-\frac{x}{\sqrt{2\tau}}}^{\infty} e^{\frac{1}{2}\left(\mu + 1\right) (x + z\sqrt{2\tau})-\frac{z^2}{2}}\:dz\\
&\frac{1}{\sqrt{2\pi}}\int_{-\frac{x}{\sqrt{2\tau}}}^{\infty} e^{-\frac{1}{2}\left(z - \sqrt{\frac{\tau}{2}}(\mu+1)\right)^2 + \frac{1}{2}\left(\mu + 1\right) x + \frac{1}{4}\tau(\mu+1)^2}\:dz & \text{subst.}\\
&\frac{e^{\frac{x\left(\mu + 1\right)}{2} + \frac{\tau(\mu+1)^2}{4}}}{\sqrt{2\pi}}\int_{-\frac{x}{\sqrt{2\tau}}}^{\infty} e^{-\frac{1}{2}\left(z - \sqrt{\frac{\tau}{2}}(\mu+1)\right)^2}\:dz\\
\end{align*}$$

A last change of variables is needed. Set

$$\begin{align*}
y &:= z - \sqrt{\frac{\tau}{2}}(\mu+1)\qquad\Longrightarrow\qquad dy = dz\\
z &:= - \frac{x}{\sqrt{2\tau}}
\end{align*}$$

and substitute, to obtain

$$\frac{e^{\frac{x \left(\mu + 1\right)}{2} + \frac{\tau (\mu+1)^2}{4}}}{\sqrt{2\pi}}\int_{-\frac{x}{\sqrt{2\tau}} - \sqrt{\frac{\tau}{2}}(\mu+1)}^{\infty} e^{-\frac{y^2}{2}}\:dy$$

The same procedure of extracting the constant term and changing the variables must be performed for the second integral as well. The result of substituting both rewritten integrals into (11) is

$$\begin{align*}
u(x,\tau) &= \frac{e^{\frac{x \left(\mu + 1\right)}{2} + \frac{\tau (\mu+1)^2}{4}}}{\sqrt{2\pi}}\int_{-\frac{x}{\sqrt{2\tau}} - \sqrt{\frac{\tau}{2}}(\mu+1)}^{\infty} e^{-\frac{y^2}{2}}\:dy - \frac{e^{\frac{x \left(\mu - 1\right)}{2} + \frac{\tau (\mu-1)^2}{4}}}{\sqrt{2\pi}}\int_{-\frac{x}{\sqrt{2\tau}} - \sqrt{\frac{\tau}{2}}(\mu-1)}^{\infty} e^{-\frac{y^2}{2}}\:dy\\
&= \frac{e^{\frac{x \left(\mu + 1\right)}{2} + \frac{\tau (\mu+1)^2}{4}}}{\sqrt{2\pi}}\int^{\frac{x}{\sqrt{2\tau}} + \sqrt{\frac{\tau}{2}}(\mu+1)}_{-\infty} e^{-\frac{y^2}{2}}\:dy - \frac{e^{\frac{x \left(\mu - 1\right)}{2} + \frac{\tau (\mu-1)^2}{4}}}{\sqrt{2\pi}}\int_{-\frac{x}{\sqrt{2\tau}} - \sqrt{\frac{\tau}{2}}(\mu-1)}^{\infty} e^{-\frac{y^2}{2}}\:dy\\
\end{align*}$$

where in the second step we used the symmetry of the integral to exchange the integration limits. By defining

$$\begin{align*}
d_1 &:= \frac{x}{\sqrt{2\tau}} + \sqrt{\frac{\tau}{2}}(\mu+1)\\
d_2 &:= \frac{x}{\sqrt{2\tau}} + \sqrt{\frac{\tau}{2}}(\mu-1)
\end{align*}$$

and recalling that the CDF of a Normal distribution is given by

$$\mathcal{N}(d) = \frac{1}{\sqrt{2\pi}}\int_{-\infty}^d e^{-\frac{y^2}{2}}\:dy$$

we can write

$$u(x,\tau) = e^{\frac{x \left(\mu + 1\right)}{2} + \frac{\tau (\mu+1)^2}{4}}\mathcal{N}(d_1) - e^{\frac{x \left(\mu - 1\right)}{2} + \frac{\tau (\mu-1)^2}{4}}\mathcal{N}(d_2) \tag{12}$$

We need to unwind the variable substitution that introduced \(u\). Therefore, equation (12) and the value of \(\alpha\) and \(\beta\) must be substituted in (5), yielding

$$\begin{align*}
v(x,\tau) &= e^{\alpha x + \beta\tau}\:u(x,\tau)\\
&= e^{-\frac{x\left(\mu-1\right)}{2} - \frac{\tau\left(\mu+1\right)^2}{4}}\:\left(e^{\frac{x \left(\mu + 1\right)}{2} + \frac{\tau (\mu+1)^2}{4}}\mathcal{N}(d_1) - e^{\frac{x \left(\mu - 1\right)}{2} + \frac{\tau (\mu-1)^2}{4}}\mathcal{N}(d_2)\right) & \text{subst.}\\
&= e^{\left(-\frac{\left(\mu-1\right)}{2} + \frac{\left(\mu+1\right)}{2}\right) x + \left(-\frac{\left(\mu+1\right)^2}{4} + \frac{\left(\mu+1\right)^2}{4}\right)\tau}\mathcal{N}(d_1) - e^{\left(-\frac{\left(\mu-1\right)}{2} + \frac{\left(\mu-1\right)}{2}\right) x + \left(-\frac{\left(\mu+1\right)^2}{4} + \frac{\left(\mu-1\right)^2}{4}\right)\tau}\mathcal{N}(d_2)\\
&= e^{\left(-\frac{\left(\mu-1\right)}{2} + \frac{\left(\mu+1\right)}{2}\right) x}\mathcal{N}(d_1) - e^{\left(-\frac{\left(\mu+1\right)^2}{4} + \frac{\left(\mu-1\right)^2}{4}\right)\tau}\mathcal{N}(d_2) & \text{simplifying}\\
&= e^{\frac{1}{2}\left(-\mu+1+\mu+1\right) x}\mathcal{N}(d_1) - e^{\frac{1}{4}\left(\mu^2+1-2\mu-\mu^2-1-2\mu\right)\tau}\mathcal{N}(d_2)\\
&= e^{x}\mathcal{N}(d_1) - e^{-\mu\tau}\mathcal{N}(d_2) \tag{13}\\
\end{align*}$$

To complete the unwind of variable substitutions, use equation (13) along with the expressions for \(\mu\), \(\tau\) and \(x\) into equation (2).

$$\begin{align*}
V(S,t) &= K\:v(x,\tau)\\
&= K \left(e^{x}\mathcal{N}(d_1) - e^{-\mu\tau}\mathcal{N}(d_2)\right) & \text{subst. (13)}\\
&= K\:e^{\ln\left(\frac{S}{K}\right)}\mathcal{N}(d_1) - K\:e^{-\frac{2r}{\sigma^2}\frac{1}{2}\sigma^2\left(T-t\right)}\mathcal{N}(d_2) & \text{subst. }x,\:\tau,\:\mu\\
&= K \frac{S}{K}\:\mathcal{N}(d_1) - K\:e^{-r\left(T-t\right)}\mathcal{N}(d_2)\\
&= S\:\mathcal{N}(d_1) - K\:e^{-r\left(T-t\right)}\mathcal{N}(d_2)\\
\end{align*}$$

Finally, transform also the integration limits in the same way:

$$\begin{align*}
d_1 &= \frac{x}{\sqrt{2\tau}} + \sqrt{\frac{\tau}{2}}(\mu+1)\\
&= \frac{\ln\left(\frac{S}{K}\right)}{\sqrt{2\frac{1}{2}\sigma^2\left(T-t\right)}} + \sqrt{\frac{\frac{1}{2}\sigma^2\left(T-t\right)}{2}}\left(\frac{2r}{\sigma^2}+1\right) & \text{subst. }x,\:\tau,\:\mu\\
&= \frac{\ln\left(\frac{S}{K}\right)}{\sigma\sqrt{T-t}} + \frac{\sigma}{2}\sqrt{T-t}\left(\frac{2r}{\sigma^2}+1\right)\\
&= \frac{\ln\left(\frac{S}{K}\right)}{\sigma\sqrt{T-t}} + \frac{\sigma}{2}\sqrt{T-t}\left(\frac{2r}{\sigma^2}+1\right)\frac{\sigma\sqrt{T-t}}{\sigma\sqrt{T-t}}\\
&= \frac{1}{\sigma\sqrt{T-t}}\left(\ln\left(\frac{S}{K}\right) + \frac{\sigma^2}{2}\left(T-t\right)\left(\frac{2r}{\sigma^2}+1\right)\right)\\
&= \frac{1}{\sigma\sqrt{T-t}}\left(\ln\left(\frac{S}{K}\right) + r\left(T-t\right) + \frac{\sigma^2}{2}\left(T-t\right)\right)\\
\end{align*}$$

Similarly for \(d_2\):

$$\begin{align*}
d_2 &= \frac{x}{\sqrt{2\tau}} + \sqrt{\frac{\tau}{2}}(\mu-1)\\
&= \frac{\ln\left(\frac{S}{K}\right)}{\sqrt{2\frac{1}{2}\sigma^2\left(T-t\right)}} + \sqrt{\frac{\frac{1}{2}\sigma^2\left(T-t\right)}{2}}\left(\frac{2r}{\sigma^2}-1\right) & \text{subst. }x,\:\tau,\:\mu\\
&= \frac{1}{\sigma\sqrt{T-t}}\left(\ln\left(\frac{S}{K}\right) + \frac{\sigma^2}{2}\left(T-t\right)\left(\frac{2r}{\sigma^2}-1\right)\right)\\
&= \frac{1}{\sigma\sqrt{T-t}}\left(\ln\left(\frac{S}{K}\right) + r\left(T-t\right) - \frac{\sigma^2}{2}\left(T-t\right)\right)\\
\end{align*}$$

### Final remark

To get from equation (10) to equation (11), the initial condition in equation (9) has been used. This is the initial condition of a _call_ option as it can be seen from equation (8). This yielded the resulting Black-Scholes solution for a call option. To obtain the solution of a put option, equations (8) and (9) must be rewritten accordingly to a put option yielding a different equation (11). A part from this change, the rest of the calculation for a put option proceeds identically.
