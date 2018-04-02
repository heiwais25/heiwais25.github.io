---
title: How to choose hyper parameter
layout: post
author:     "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [dataAnalysis, statistics]
---
<p>
	We already dealt with hyper parameter in previous several post. It is parameter of prior distribution. The prior distribution is influenced by how we choose these hyper parameters. There are two main startegies how to choose hyper parameter
</p>
### Strategy 1
<p>
	In this strategy, we decide hyper parameter based on our <b>personal knowledge.</b> We can choose proper number by considering how confident we will be when we have n more data points or how many units of information we think we have to include in our prior. For example, let's consider the number of chocolate chips per cookies on average. It is poisson distribution and we can write prior mean and other like this.
	$$\text{Prior mean :}\frac{\alpha}{\beta}$$
	$$\text{Prior std.dev : }\frac{\sqrt{\alpha}}{\beta}$$
	$$\text{Effective sample size : }\beta$$
</p>
### Strategy 2
<p>
	<b>The purpose of this strategy is decreasing the effective sample size to minimize the influence from prior to posterior.</b> So, we will set prior to vague prior epsilon.
	$$\epsilon \gt 0, \Gamma(\epsilon, \epsilon)$$
	$$\text{Prior mean :} 1$$
	$$\text{Prior std.dev :} \frac{1}{\epsilon}$$
	$$\text{Posterior mean : }\frac{\epsilon + \Sigma y_i}{\epsilon + n} \sim \frac{\Sigma y_i}{n}$$
</p>