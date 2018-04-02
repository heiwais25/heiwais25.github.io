---
title: Normal distribution prior
layout: post
author:     "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [dataAnalysis, statistics]
---
<p>
	There are two cases for the prior of normal distribution because it has two parameters(mean, std.dev). The first case is when we know std.dev and second case is when we don't know std.dev. <b>Normally, normal distribution is applied to continuous quantity and certain case central limit theorem can be applied.</b>
</p>
### Normal distribution with variance known
<p>
	In this case, what we want to do is calculating the mean value. 
	$$X_i \sim N(\mu, \sigma^2)$$
	Conjugate of normal distribution is itself of mean distribution.
	$$\text{prior : } \mu \sim N(m_0, s_0^2)$$
	$$f(\mu|X) \varpropto f(\tilde x | \mu)f(\mu)$$
	Through some calculation, we can get result like this.
	$$\mu|\tilde x \sim N(\frac{\frac{n\bar x}{\sigma_0^2}+\frac{m_0}{s_0^2}}{\frac{n}{\sigma_0^2}+\frac{1}{s_0^2}},\frac{1}{\frac{n}{\sigma_0^2}+\frac{1}{s_0^2}})$$
	$$\text{Posterior mean : } \frac{\frac{n}{\sigma_0^2}}{\frac{n}{\sigma_0^2}+\frac{1}{s_0^2}}\bar x + \frac{\frac{1}{\sigma_0^2}}{\frac{n}{\sigma_0^2}+\frac{1}{s_0^2}}m$$
	$$= \frac{n}{n+\frac{\sigma_0^2}{s_0^2}}\bar x +\frac{\frac{\sigma_0^2}{s_0^2}}{n+\frac{\sigma_0^2}{s_0^2}}m$$
	$$\text{Effective sample size : } \frac{\sigma_0^2}{s_0^2}$$
</p>
### Normal distribution with variance unknown
<p>
	Another case is when we don't know variance also. <b>Unlike the conjugate prior for mean value was normal distribution, conjugate prior for sigma square is inverse-gamma distribution.</b> As we can see in the name, inverse-gamma distribution is related to gamma distribution. Below equation is showing relation with gamma distribution and inverse gamma distribution.
	$$X \sim \Gamma(\alpha, \beta)$$
	$$Y = \frac{1}{X} \sim \Gamma^{-1}(\alpha, \beta)$$
	$$f(y) = \frac{\beta^\alpha}{\Gamma(\alpha)}y^{-(\alpha+1)}e^{-\frac{\beta}{y}}I_{y>0}$$
	$$E(Y)=\frac{\beta}{\alpha-1}\text{ for }\alpha \gt 1$$
	We can use this prior to get marginal posterior distribution when the variance is unknown.
	$$X_i|\mu, \sigma^2 \sim N(\mu, \sigma^2)$$
	$$\mu|\sigma^2 \sim N(m, \frac{\sigma^2}{w})$$
	$$\text{Effective sample size : } w=\frac{\sigma^2}{\sigma_{\mu}^2}$$
	$$\sigma^2 \sim \Gamma^{-1}(\alpha, \beta)$$
	$$\sigma^2|\tilde x \sim \Gamma^{-1}(\alpha+\frac{n}{2},\beta+\frac{1}{2}\Sigma_{i=1}^n (x_i - \bar x)^2 + \frac{nw}{2(n+w)}(\bar x - m)^2)$$
	$$\mu|\sigma^2, \tilde x \sim N(\frac{n\bar x + wm}{n+w},\frac{\sigma^2}{n+w})$$
	$$\frac{n\bar x + wm}{n+w}=\frac{w}{n+w}m+\frac{n}{n+w}\bar x$$
	$$\mu|\tilde x \sim \text{ follows t distribution}$$
</p>
#### Supplement
<p>
	<b>t distribution or student's t-distribution is any member of a family of continuous probability distribution that arises when estimating the mean of a normally distributed population in situation when the sample size is small and population standard deviation is unknown.(from <a href="https://en.wikipedia.org/wiki/Student's_t-distribution">Wiki</a>)</b>
	Let's consider case that there are n identically distributed as normal distribution with expected value and variance. Then,
	$$\text{Sample mean : }\bar X = \frac{1}{n}\Sigma_{i=1} ^n X_i$$
	$$\text{Sample variance : }S^2 = \frac{1}{n-1}\Sigma_{i=1} ^n (X_i - \bar X)^2$$
	Then the each random variable follows each distribution
	$$\text{Standard normal distribution : } \frac{\bar X - \mu}{\sigma/\sqrt{n}}$$
	$$\text{Student's t-distribution with n-1 degrees of freedom : } \frac{\bar X - \mu}{S/\sqrt{n}}$$
	The definition of probability density function of t-distribution is like this.
	$$f(t) = \frac{\Gamma(\frac{\nu + 1}{2})}{\sqrt{\nu \pi}\Gamma(\frac{\nu}{2})}(1+\frac{t^2}{\nu})^{-\frac{\nu+1}{2}}$$
</p>