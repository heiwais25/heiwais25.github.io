---
title: Probability distribution in R(2)
layout: post
author:     "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [R, dataAnalysis, statistics]
---
<p>
	As we can see in the previous <a href="/r/dataanalysis/statistics/2017/09/12/probability-distribution-in-r1/"></a>post, we can calculate and plot distribution easily. Because there are so many distribution function, Let's deal with other distribution example. 
</p>
$$\begin{array}{c|c|c}
\text{Distribution}&\text{Function in R}&\text{Parameters}\\
Binomial(n,p)&dbinom(x, size, prob)&size = n, prob = p\\
Poisson(\lambda)&dpois(x, lambda)&lambda = \lambda \\
Exp(\lambda)&dexp(x, rate) & rate = \lambda \\
Gamma(\alpha,\beta)&dgamma(x, shape, rate)&shape=\alpha,\; rate=\beta\\
Uniform(a,b)&dunif(x,min,max)&min = a,\;max=b\\
Beta(\alpha, \beta)&dbeta(x,shape1,shape2)&shape1 = \alpha,\;shape2 = \beta\\
N(\mu, \sigma^2)&dnorm(x,mean,std.dev)&mean=\mu,\;std.dev=\sqrt(\sigma^2)\\
t_{\nu}&dt(x,df)&df=\nu\\
\end{array}$$
<p>
	By using these distribution, we can calculate what we want. The other application like CDF, quantile function and random pseudo generator can be used by substituting first char to p, q, r.
</p>
<p>
	Let's do some practice. 
	$$1.\;X\sim Pois(2)\text{Find } P(X=2) $$
	$$2.\;X\sim Pois(2)\text{Find } P(X \ge 2) $$
	$$3.\;X\sim Pois(2)\text{Find } P(X \le 2) $$
	$$4.\;X\sim Gamma(2, \frac{1}{2})\text{Find } P(0.5 \le X \le 2) $$
	$$5.\;X\sim N(0,1)\text{Find the z such that } P(X \le z) = 0.8$$
	$$6.\;X\sim N(0,1)\text{Find } P(-1.96 \le X \le 1.96)$$
	$$7.\;X\sim N(0,1)\text{Find the z such that } P(-z \le X \le z) = 0.98$$
</p>
<p>
	Solution is like this.
<pre><code>1. P(x=2) : dpois(2,2) = 0.271
2. P(X>2) : 1 - ppois(2,2) = 0.323
3. P(X<2) : ppois(2,2) = 0.677
4. P(0.5<X<2) : pgamma(2,2,1/2) - pgamma(0.5,2,1/2) = 0.238
5. P(X<z) = 0.8 : qnorm(.8, 0, 1) = 0.842
6. P(-1.96 < X < 1.96) : pnorm(1.96, 0, 1) - pnorm(-1.96, 0, 1) = 0.950
7. P(-z < X < z) = 0.98 : qnorm(0.99, 0, 1) = 2.32 # Due to its symmetry</code></pre>
</p>