---
title: Conjugate Priors
layout: post
author:     "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [dataAnalysis, statistics]
---
### Bernouilli / Binomial distribution likelihood with uniform prior
<p>
	As we can see in this <a href="/dataanalysis/statistics/2017/09/05/predictive-interval-2/">post</a>, the output or posterior will be beta distribution if we use uniform prior to bernouilli / binomial distribution. The generalization of bernouilli / binomial distribution with uniform prior is like this.
	$$f(\tilde y|\theta)=\theta^{\Sigma y_i}(1-\theta)^{n-\Sigma y_i}\;f(\theta)=I_{0 \le \theta \le 1}$$
	$$f(\theta | \tilde y) = \frac{f(\tilde y|theta)f(\theta)}{\int f(\tilde y|theta)f(\theta)d\theta} = \frac{\theta^{\Sigma y_i}(1-\theta)^{n-\Sigma y_i}I_{0 \le \theta \le 1}}{\int \theta^{\Sigma y_i}(1-\theta)^{n-\Sigma y_i}I_{0 \le \theta \le 1} d\theta}$$
	$$=\frac{\theta^{\Sigma y_i}(1-\theta)^{n-\Sigma y_i}I_{0 \le \theta \le 1}}{\frac{\Gamma(\Sigma y_i + 1)\Gamma(n-\Sigma y_i + 1)}{\Gamma(n+2)}\int^1_0 \frac{\Gamma(n+2)}{\Gamma(\Sigma y_i +1)\Gamma(n- \Sigma y_i +1)}\theta^{y_i}(1-\theta)^{n-\Sigma y_i}d\theta}$$
	$$=\frac{\Gamma(n+2)}{\Gamma(\Sigma y_i +1)\Gamma(n - \Sigma y_i +1)}\theta^{\Sigma y_i}(1-\theta)^{n-\Sigma y_i}I_{0 \le \theta \le 1}$$
</p>
<p>
	Through the previous equation, we can see posterior for theta given y follows a beta distribution. We can use this relation to make general sentense. <b>Actually, the uniform distribution is a kind of beta distribution of parameter 1,1 and any beta distribution is conjugate for the bernouilli distribution.</b> 
	$$f(\theta)=\frac{\Gamma(\alpha + \beta)}{\Gamma(\alpha)\Gamma(\beta)}\theta^{\alpha-1}(1-\theta)^{\beta -1}I_{0 \le \theta \le 1}$$
	$$f(\theta|\tilde y) \varpropto f(\tilde y|\theta)f(\theta)=\theta^{\Sigma y_i}(1-\theta)^{n-\Sigma y_i}\frac{\Gamma(\alpha+\beta)}{\Gamma(\alpha)\Gamma(\beta)}\theta^{\alpha-1}(1-\theta)^{\beta-1}I_{0 \le \theta \le 1}$$
	$$\varpropto \theta^{\alpha+\Sigma y_i -1}(1-\theta)^{\beta + n - \Sigma y_i -1}I_{0 \le \theta \le 1}$$
	$$\text{So, }f(\theta|\tilde y) \varpropto beta(\alpha + \Sigma y_i, \beta + n - \Sigma y_i)$$
	<b>If the prior distribution has alpha and beta 1, it starts from uniform distribution.</b> In this calculation, the order of event is not important. What we need to consider terms which includes parameter theta.
</p>
<p>
	Now, we can say <b>conjugae family. It is a family of distribution is referred to as conjugate if when we use member of that family as a prior, we get another member of that familiy as our posterior.</b> In the previous case, the beta distribution is conjugate for the bernouilli distribution. Because this relationship makes calculation much simple, it is better to use conjugate distribution as a prior. One thing we need to know is that when we use beta distribution or other distribution as a prior, there are another parameter like alpha and beta. We call this parameter as hyper parameter.
</p>
### Posterior mean and effective sample size
<p>
	In the case of bernouilli likelihood with N observation using beta prior, now we can write the posterior predictive distribution is like this. Also, we can set effective sample size using hyper parameter.
	$$\text{prior} \sim beta(\alpha, \beta)$$
	$$\text{effective sample size} = \alpha + \beta$$
	$$\text{posterior} \sim beta(\alpha + \Sigma y_i, \beta +n - \Sigma y_i$$
	$$\text{mean of beta} = \frac{\alpha}{\alpha + \beta}$$
	$$\text{posterior mean} = \frac{\alpha + \Sigma y_i}{\alpha + \beta + n} = \frac{\alpha+\beta}{\alpha+\beta+n}\frac{\alpha}{\alpha+\beta}+\frac{n}{\alpha+\beta+n}\frac{\Sigma y_i}{n}$$
	$$=\text{prior weight} \cdot \text{prior mean} + \text{data weight} \cdot \text{data mean}$$
	<b>In this calculation, effective sample size describes that how many data is required to be confident that prior does not affect the result of posterior. So, we can conclude the posterior is not affected from prior by comparing effective sample size and the number of event n.</b>
</p>
<p>
	If we observe event in different time, it is not problem in bayesian paradigm. The only thing need to do is updating posterior sequentially. Because we update it sequentially, it has identical result whether we perform at one time or seperately. In the case of frequentist paradigm, it is different case if we perform in different days.
</p>
