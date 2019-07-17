---
title: Non-informative priors
layout: post
author:     "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [Data Analysis, statistics]
---
<p>
	There are another method trying to decrease the effect of prior and maximize the effect of data. It is called non-informative priors. What is informative or non-informative? We can see this kind of example to understand non-informative. The question in below is how do we minimize our informaition on theta.
	$$Y_i \sim B(\theta)$$
</p>
<p>
	As we did in previous post, Let's start from eqaully likely prior. It seems there are no information because it has uniform distribution. However in several case, it has information although it is uniform distribution
	$$\theta \sim U[0,1] = beta(1,1)$$
	The beta distribution with 1 and 1 has the information that it's sample size is 2(=1+1). Then we can think of beta(1/2,1/2) or beta(.001,.001) which has small sample size. For the extreme case, we can think of beta(0,0). Actually, it is not probability density function because it diverge when we integrate to see normalization condition.
	$$beta(0,0)$$
	$$f(\theta) \varpropto \theta^{-1}(1-\theta)^{-1}$$
	It is called <b>improper prior</b>. Although it is improper prior, we can use this if the posterior is proper. In this bernouilli case, it is okay if we can see at least one success and one failure.
	$$f(\theta|y) \varpropto \theta^{y-1}(1-\theta)^{n-y-1} \sim beta(y,n-y)$$
	$$\text{Posterior mean : } \frac{y}{n}=\hat \theta$$
	<b>Many problems that there does exist a prior, typically an improper prior, that will lead to same point estimates as you would get under frequentist paradigm</b>
</p>
<p>
	Let's see another example of normal distribution case. In this case, the non-informative prior can be vague prior which has very large std.dev. It is possible to use infinite variance. We can deal with this kind of distribution as uniform distribution. First case is when we know variance.
	$$Y_i \sim N(\mu,\sigma^2)$$
	$$f(\mu|y) \varpropto f(y|\mu)f(\mu) \varpropto exp[-\frac{1}{2\sigma^2}\Sigma(y_i - \mu)^2](1)$$
	$$\varpropto exp[-\frac{1}{2\sigma^2/n}(\mu - \bar y)^2]$$
	$$\mu|\tilde y \sim N(\bar y, \frac{\sigma^2}{n}$$
	Second case is when we don't know the variance.
	$$f(\sigma^2) \varpropto \frac{1}{\sigma^2}\Gamma^{-1}(0,0)$$
	$$\sigma^2|\tilde y \sim \Gamma^{-1}(\frac{n-1}{2},\frac{1}{2}\Sigma(y_i-\bar y)^2)$$
</p>
<p>
	As you can see in several example, it is same result with when we calculate this from the frequentist paradigm. <b>The reason why we use like this in bayesian paradigm is we can calculate posterior probability and credible interval.</b>
</p>