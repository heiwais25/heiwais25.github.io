---
title: Jeffreys prior
layout: post
author:     "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [Data Analysis, statistics]
---
<p>
	When we consider choosing prior, there might be different prior but same kind of distribution. Although it is same kind of distribution, they will lead us to different posterior. Let's see uniform distribution example in the normal distribution. 
	$$Y_i \sim N(\mu, \sigma^2)$$
	$$\text{Two priors 1 : }f(\sigma^2)\varpropto\frac{1}{\sigma^2} \text{ 2 : } f(\sigma^2) \varpropto 1$$
	These two prior are in uniform distribution but will make different posterior. If we do parameter transformation to this uniform distribution, it will make different posterior. This kind of distribution is called <b>non-invariant</b>.
</p>
<p>
	To deal with invariant distribution under parameter transformation, we can use <a href="https://en.wikipedia.org/wiki/Jeffreys_prior">Jeffreys prior</a> which is using fisher information. 
	$$f(\theta) \varpropto \sqrt{I(\theta)}$$
	After some calculation, we can get Jeffreys prior in each distribution.
	$$Y_i \sim B(\theta)\;f(\theta)\varpropto Beta(\frac{1}{2},\frac{1}{2})$$
	$$Y_i \sim N(\mu, \sigma^2)\;f(\mu)\varpropto1,\;f(\sigma^2)\varpropto\frac{1}{\sigma^2}$$
	$$Y_i \sim Poisson(\lambda)\;f(\lambda)\varpropto \frac{1}{\lambda}$$
	<b>These Jeffreys prior will make same information under parameterization. It means it will lead us to same posterior.</b>
</p>
