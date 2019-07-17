---
title: brief introduction of distribution
layout: post
author:     "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [Data Analysis, statistics]
---
## Discrete parameter case
### 1) Bernouilli distribution
<p>
	&nbsp;&nbsp;&nbsp;If the random variable X written like below equation which has probability p, we can say this is Bernouilli distribution. 
	$$X \thicksim B(p)$$ 
	&nbsp;&nbsp;&nbsp;It's used when we have two possible outcomes like flipping coin(success, failure). For example, there are only two cases
	head or tail if we flip coin. Then, the probability of each can be written like this.
	$$Success\,or\,head\,:\,P(X\,=\,1)\,=\,p$$ $$Failures\,or\,tails\,:\,P(X=0)\,=\,1\,-\,p$$ 
	&nbsp;&nbsp;&nbsp;We can simplify function of all different outcomes like this. This form is used in all distribution by changing parameter 
	in each case.$$f(X=x|p) = f(x|p)$$
	&nbsp;&nbsp;&nbsp;In the case of Bernoulli distribution we can write typical distribution like this including indicator function. $$f(x|p)=p^x(1-p)^{(1-x)}I_{\{x\in\{0,1\}\}}$$
	&nbsp;&nbsp;&nbsp;The expectation value and variance of Bernouilli distribution can be calculated like this. When we have N repeated case, 
	we can get generalization form of Bernouilli distribution which is binomial distribution
	$$E[X]=\Sigma xP(X=x) = p$$ $$Var[X]=p(1-p)$$
</p>
### 2) Binomial distribution
<p>
	&nbsp;&nbsp;&nbsp;When the random variable X follows binomial distribution, we can write as follows. Also expectation value and variance can
	be calculated because it is just extension of Bernouilli distribution.
	$$X \thicksim Binomial(n,p)$$
	$$P(X=x|p)\,=\,f(x|p)\,=\,\binom n x p^x(1-p)^{n-x}$$
	$$E[X]=np$$
	$$Var[X]=np(1-p)$$
</p>
## Continuous case
### 1) Uniform distribution
<p>
	&nbsp;&nbsp;&nbsp;In the continuous case like uniform distribution, we can not calculate probability at each point(It will be 0). If we want to calculate the 
	probability, we need to integrate probability density in certain range. Then we can know how probable we get the certain range of value. The important thing in 
	here is the result should be 1 when we integrate probability density in whole possible value.
	&nbsp;&nbsp;&nbsp;The uniform distribution is the case of random variable distributed uniformly in the certain range. For the simplicity, we can think of 
	uniform distribution in the [0,1]. Then, we can write as follows.
	$$X \thicksim U[0,1]$$
	$$density f(x) = I_{\{0\leq x \leq 1\}}(x)$$
</p>
### 2) Exponential distribution
<p>
	&nbsp;&nbsp;&nbsp;The exponential distribution is typicall used for related waiting time like waiting for bus. The parameter in this distribution is lambda which
	is the rate how frequently event occur. The probability density, expectation value and variance can be written as follows.
	$$f(x|\lambda)=\lambda\exp[-\lambda x]$$
	$$E[X]=\frac 1 \lambda$$
	$$Var[X] =\frac 1 {\lambda^2}$$
</p>
### 3) Normal distribution(Gaussian distribution)
<p>
	&nbsp;&nbsp;&nbsp;Thanks to Central Limit Theorem, we can use this normal distribution in many cases with many events. Also, we can calculate <b>confidence interval</b> 
	using this. It has parameter of mean value and standard deviation.
	$$X~[\mu,{\sigma^2}]$$
	$$f(x|\mu,\sigma)=\frac 1 {\sqrt{2\pi{\sigma^2}}}\exp[- \frac 1 {2{\sigma^2}} (x-\mu)^2]$$
	$$E[X]=\mu$$
	$$Var[X]={\sigma^2}$$
</p>