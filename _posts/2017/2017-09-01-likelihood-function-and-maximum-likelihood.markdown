---
title: Likelihood function and maximum likelihood
layout: post
author:     "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [Data Analysis, statistics]
---
<p>
	&nbsp;&nbsp;&nbsp;Let's think of the case of one hospital. There are 400 patient of heart attack in the hospital. 
	After one month, 72 patients is dead and 328 patients are alive. The question is could we estimate the mortality rate?
	In here, we can check frequentist paradigm first.
</p>
### Frequentist paradigm
<p>
	&nbsp;&nbsp;&nbsp;Before dealing with the mortality rate, we need to establish our referece population. The reference population
	can be heart attack patients in the region or the number of heart attack patients in the hopistal long time. Both model can be 
	reasonable. But the problem is our 400 patient data is not random sample from either of those population model. If we want to deal
	with random situation, we need to consider every people who can go hospital and have potential to heart attack. As you can see,
	This is hypothetical case and there are philosophical issues
</p>
### Bayesian paradigm
<p>
	&nbsp;&nbsp;&nbsp;We can start from setting reasonable distribution. <b>In here, there are only two choices heart attack or not.
	So, we can use binomial distribution(also bernouilli distribution).</b> Then, the distribution and probability can be written liek this.
	In the equation, the y tilde is meaning vector from probability density function for entire set.
	$$Y_i \thicksim B(\theta)$$ 
	$$P(Y_i = 1) = \theta \, success is mortality$$
	$$P(\tilde{Y}=\tilde{y}|\theta) = P(Y_1 = y_1,...,Y_n=y_n|\theta) = P(Y_1=y_1)P(Y_2=y_2)...P(Y_n=y_n)$$
	$$= \Pi_{i=1}^n \theta^{y_i}(1-\theta)^{1-y_i}$$
</p>
### Likelihood function
<p>
	&nbsp;&nbsp;&nbsp;Now, we get probability function. We can go further with this equation to get the mortality value. The likelihood function is density
	function thought as a function of theta like this.
	$$L(\theta | y)= \Pi_{i=1}^n \theta^{y_i}(1-\theta)^{1-y_i}$$
	&nbsp;&nbsp;&nbsp;<b>The strategy to get proper theta is choosing theta value that gives the biggest value of the likelihood. It makes the data the most
	likely to occur for the particular data we observe. It is theta that maximize likelihood(MLE) and can be written like this. </b>
	$$MLE\,\,\, \hat\theta = argmax\,L(\theta|\tilde{y})$$
	Then, only left thing is differentiate previous likelihood function and get the theta value which can make function maximum or differentiation is 0.
	If we differentiate the function and get the value, the mortality can be calculated like this. You can see this is same with calculating average of the sample.
	$$\hat\theta = \frac{1}{n} \sum y_i = \frac{72}{400} = .18$$
	Also, we can write the normal distribution form and calculate credible interval approximately like this.  
	$$\hat\theta \thicksim N(\theta, \frac{1}{I(\theta)})$$
	$$CI : \hat\theta \pm 1.96\sqrt{\frac{\hat\theta(1-\hat\theta)}{n}}$$
	In the first line, we can see I(theta). This is called fisher information. In the case of binomial distribution, the fisher information is written like this.
	$$I(\theta) = \frac{1}{\theta(1-\theta)}$$
</p>
<p>
	&nbsp;&nbsp;&nbsp;Through this method, we can calculate the parameter value which maximizing likelihood. This method can be applied to another distribution also like exponential
	distribution. 
</p>

