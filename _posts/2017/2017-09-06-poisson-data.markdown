---
title: Poisson / Exponential distribution
layout: post
author:     "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [Data Analysis, statistics]
---
### Poisson distribution
<p>
	As we can see in previous slide, we need to use <b>bernouilli / binomial</b> distribution when we deal with specific event which make only two result (success / failure). In the case of dealing with another event like how many cholcolate chips distributed in each cookies, we need to use <b>poisson distribution</b>.
</p>
<p>
	Poisson distribution has one parameter lambda. We can also think of conjugate distribution of poisson distribution. That is <b>gamma distribution.</b> 
	$$Y_i \sim \text{Pois}(\lambda)$$
	$$f(\tilde y|\lambda) = \frac{\lambda^{\Sigma y_i}e^{-n\lambda}}{\Pi^n_{i=1}y_i!} \text{for} \lambda \gt 0$$
	$$\text{Gamma prior}\;\lambda \sim \Gamma(\alpha, \beta)$$
	$$f(\lambda)=\frac{\beta^\alpha}{\Gamma(\alpha)}\lambda^{\alpha-1}e^{-\beta \lambda}$$
	$$f(\lambda|\tilde y) \varpropto f(\tilde y|\lambda)f(\lambda) \varpropto \lambda^{\Sigma y_i}e^{-n\lambda}\lambda^{\alpha-1}e^{-\beta \lambda}=\lambda^{(\alpha+\Sigma y_i)-1}e^{-(n+\beta)\lambda}$$
	$$\text{Posterior}\;\Gamma(\alpha+\Sigma y_i,\beta + n)$$
</p>
<p>
	Mean of prior gamma distribution and posterior is like this
	$$\text{Prior mean}\;\frac{\alpha}{\beta}$$
	$$\text{Posterior mean}\;\frac{\alpha+\Sigma y_i}{\beta + n}=\frac{\beta}{\beta+n}\frac{\alpha}{\beta}+\frac{n}{\beta+n}\frac{\Sigma y_i}{n}$$
	$$\text{Effective sample size : } \beta$$
</p>
### Exponential distribution
<p>
	When we wait for bus which is coming every 10 minute averagely, we can ues <b>exponential distribtuion</b> when bus will come. The parameter of distribution is lambda which is showing how oftern the event will happer. The important thing is <b>gamma distribution is also conjugate of exponential distribution.</b>
</p>
<p>
	For example, if the bus come every 10 minutes averagely, the parameter of exponential distribution is 1 over 10.
	$$Y \sim \text{Exp}(\lambda)$$
	$$\text{Every 10 minutes : } \lambda = \text{Prior mean}=\frac{1}{10}$$
	$$\text{Prior mean is same with }\Gamma(100,1000)$$
	$$\text{Prior std.dev }= \frac{1}{100}$$
	$$\text{mean} \pm \text{two std.dev }= 0.1 \pm 0.02$$
</p>
<p>
	When there are one bus coming after 12 minutes, we can update posterior with our new data.
	$$f(\lambda|y) \varpropto f(y|\lambda)f(\lambda) \varpropto \lambda e^{-\lambda y}\lambda^{\alpha -1}e^{-\beta\lambda}=\lambda^{(\alpha+1)-1}e^{-(\beta+y)\lambda}$$
	$$\text{Posterior } \Gamma(\alpha +1, \beta + y)$$
	$$\text{So, } f(\lambda|y) \sim \Gamma(100,1012)$$
	$$\text{Posterior mean }= \frac{101}{1012} = \frac{1}{10.02}$$
	We can see waiting time is larger than 10 minutes which is prior value. However, there are no big affect from one data.
</p>

