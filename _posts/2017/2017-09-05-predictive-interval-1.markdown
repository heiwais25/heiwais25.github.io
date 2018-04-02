---
title: Predictive interval 1
layout: post
author:     "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [dataAnalysis, statistics]
---
<p>
	Let's start from the binomial example which is the simplest one. The example is how many heads predictive to see when we flip coin ten times and count the number of heads. To proceed the calculation, we need to choose priors of theta distribution.
	$$X=\Sigma^10_{i=1}Y_i$$
	$$f(\theta)=I_{0 \le \theta \le 1}$$
	$$f(x) = \int f(x|\theta)f(\theta)d\theta = \int^1_0 \frac{10!}{x!(10-x)!}\theta^x(1-\theta)^{10-x}d\theta$$
	To calculate this integral, we need new distribution function, called <b>beta distribution including gamma function</b>
</p>
<p>
	In brief, the gamma function is generalization of factorial function. For the integer case, it behaves simliar with factorial functon. Also, we can write beta distribution using gamma function.
	$$n!=\Gamma(n+1)$$
	$$z \sim beta(\alpha, \beta)$$
	$$f(z) = \frac{\Gamma(\alpha+\beta)}{\Gamma(\alpha)\cdot\Gamma(\beta)}\cdot z^{\alpha-1}(1-z)^{\beta-1}$$
	Let's apply this distribution to our 10 flipping coin case. <b>The advangtage of this approach is we can use the normalization characteristics of beta distribution like below equation.</b> Through this method, we can make integral to 1 and calculate only simple one.
	$$f(x)=\int^1_0 \frac{\Gamma(11)}{\Gamma(x+1)\Gamma(11-x)}\theta^{(x+1)-1}(1-\theta)^{(11-x)-1}d\theta$$
	$$=\frac{\Gamma(11)}{\Gamma(12)}\int^1_0\frac{\Gamma(12)}{\Gamma(x+1)\Gamma(11-x)}\theta^{(x+1)-1}(1-\theta)^{(11-x)-1}d\theta$$
	$$=\frac{\Gamma(11)}{\Gamma(12)}=\frac{10!}{11!}=\frac{1}{11}\,for\,x\in\{0,1,2,...,10\}$$
</p>
<p>
	As we can see in the result of previous calculation, we start from uniform prior distribution of parameter and get uniform predictive density for x. <b>It means if all possible coins or all possible probabilities are equally likely, then all possible x outcomes are equally likely.</b>
</p>