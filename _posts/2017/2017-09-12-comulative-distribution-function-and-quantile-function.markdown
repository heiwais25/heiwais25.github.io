---
title: Cumulative distribution function and Quantile function
layout: post
author:     "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [Data Analysis, statistics]
---
### Cumulative distribution function
<p>
	Cumulative distribution function(CDF) can be used for any distribution function including discrete and continuous function. We can think of this function behaves as we can see in this name. To calculate this function, we need to sum over from the lowest value to certain point. It can be represented like this.
	$$\text{CDF : }F(x)=P(X \le x)$$
	To calculate it more in detail, we can divide it into two case(discrete, continuous).
	$$\text{1. Discrete case : }F(x)=\Sigma_{t=-\infty}^x f(t) \text{where }f(t)=P(X=t)$$
	$$\text{2. Continuous case : }F(x)=\int_{t=-\infty}^x f(t)dt \text{where f(t) is PDF}$$
</p>
<p>
	If we know CDF function, it is also easy to calculate the probabilities of interval like this.
	$$P(a \le X \le b) = P(X\le b) - P(X\le a)$$
</p>
### Quantile function
<p>
	Normally, function related to probability is used to calculate probability at some point. Unlike other function, <b>quantile function</b> is used to find point which is satisfying certain probability. We can start with setting probability p which we want between 0 and 1. Next step is using CDF. We want to find CDF of x which is satisfying probability p. It can be written into this equation.
	$$P(X\le x) = p$$
	<b>This value x satisfying probability p is called p quantile(or 100p percentile) of the distribution of X.</b>
</p>
<p>
	Let's do some exercises. In your class, you found the 95th percentile of scores among your class mates is 75. Then, what information can you get? It means that if you want to get score higher than 95% of your class mates, you need to get score higher than 75. It can represent like this.
	$$P(X<75) = .95$$
</p>
<p>
	Other example is dealing with normal distribution. In the case of normal distribution with 0 mean and 1 std.dev, how can we calculate middle 50% of probability point? To solve this, we need .25 quantile and .75 quantile as we can see in CDF definition. After calculating .25 quantile and .75 quantile, we can substract them to get 50% of probability
	$$\text{Standard normal distribution } Z \sim N(0,1)$$
	$$\text{.25 quantile : }-0.674\;\text{ .75 quantile : }0.674$$
	$$P(-0.674 \lt Z \lt 0.674) = 0.5$$
	Like these example, we can calculate which value is required to get certain probability
</p>