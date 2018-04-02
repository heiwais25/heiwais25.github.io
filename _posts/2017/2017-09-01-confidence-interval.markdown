---
title: Confidence interval
layout: post
author:     "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [dataAnalysis, statistics]
---
<p>
	&nbsp;&nbsp;&nbsp;Let's assume we fliped coin 100 times and get the result of 44 Head and 56 Tail. The question is can we believe this coin is fair.
	In this case, this event will follow binomial distribution. By using <b>Central Limit Theorem</b>, we can use expectation value and variance as the parameters of normal distribution. Using the charactertistics of normal distribution, we can get <b>confidence interval</b> in this case.
	$$X_i \thicksim Binom(p)$$
	$$E[X] = np = 100p$$
	$$Var[X] = np(1-p) = 100p(1-p)$$
	$$\Sigma x_i =\thicksim N(100p, 100p(1-p))$$
	$$Confidence interval\,:\, 44\pm1.96\sqrt{44(.56)}$$
	The above value 1.96 is corresponding to 95% confidence interval. So, we can say we are 95% confident p in belongs to that interval.
	Because the confidence interval includes .5 which is the coin is fair, we can say this coin is fair.
</p>
<p>
	&nbsp;&nbsp;&nbsp;The important thing is what is the exact meaning of confidence interval? What does 95% means?
	To understand it correctly, we need to divide it into two cases. <b>Frequentist paradigm</b> and <b>Bayesian paradigm</b>.
</p>
### Frequentist paradigm
<p>
	&nbsp;&nbsp;&nbsp;Let's think about infinite hypothetical sqeuence of event. In the sequence, we can calculate confidence interval every time.
	And the important point in this paradigm is that if the confidence level is 95%, then the interval will cover the true vale p averagely 95%.
	But the problem is we can not know the true value p is included in certain interval. Because frequentist paradigm consider there are exact answer for p,
	<b>the probability whether the true value p is included in certain interval is 0 or 1 in the frequentist paradigm.</b> To know the probability, we need to use
	Bayesian paradigm
</p>
### Bayesian paradigm
<p>
	&nbsp;&nbsp;&nbsp;In this paradigm, we can also calculate interval. But the difference is how to understand the meaning of interval. If the level is
	95%, the probability whether the true value p is in certain interval is 95% based on random interpretor of an unkonwn parameter. So, we can deal with the
	probability of the interval! We can call the interval differently. Not confidence interval. <b>The credible interval!</b>
</p>