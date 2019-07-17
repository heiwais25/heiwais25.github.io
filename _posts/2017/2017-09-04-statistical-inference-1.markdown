---
title: Statistical Inference 1
layout: post
author:     "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [Data Analysis, statistics]
---
<p>
	Let's think about betting with brother. Your brother brought certain coin. You don't know that coin is loaded or fair. If coin is loaded, the probability for showing 
	head is 70%. If the coin is fair, the probability will be 50%. Your brother suggested you to flip the coin 5 times. You flipped the coin and result was 2 Head and 3 Tails. In this situation, how we can make conclude the coin is fair or not?
</p>
<p>
	First of all, we need to set parameter showing whether the coin is fair or loaded. And data will follow the binominal distribution. We can calculate likelihood function with this assumption.
	$$\theta=\{fair, loaded\}$$
	$$f(x|\theta) = \binom 5 x (\frac{1}{2})^5\,\,\,if\,\theta=fair$$
	$$\>\>=\binom 5 x (.7)^x(.3)^{5-x}\,\,\,if\,\theta=loaded$$
	$$=\binom 5 x (\frac{1}{2})^5I_{\theta=fair}+\binom 5 x (.7)^x(.3)^{5-x}I_{\theta=loaded}$$
	Then, we can calculate when there are 2 Heads event with 5 flipping the coin.
	$$f(\theta(x=2))=0.3125I_{\theta=fair}+0.1323I_{\theta=loaded}$$
	In the previous post(maximum likelihood estimation), we will choose the parameter which will make the likelihood biggest. <b>In this case, we can see the assumption of fair coin is the maximum likelihood case. So, it seems the coin is fair. However, we can not say how much we can be sure with this data in the frequentist paradigm. </b>
</p>
<p>
	Let's change our question little bit. What is the probability that the coin is fair given that the coin shows 2 heads. <b>Because the coin is physical and fixed coin, it has fixed probability of coming up heads.</b> So, the result will be like this in the frequentist paradigm.
	$$P(\theta=fair|x=2)=P(\theta=fair)=0\>\>or\>\>1$$
</p>
<p>
	On the other hand, the bayseian paradigm, It has advantage that it is easy to add the result of observation. First of all, we need to set the prior probability. In this case, it can be how much we believe our brother. Let's set the probability that brother gave loaded coin and update our prior with the data to get our posterior beilief using bayesian theorm
	$$P(\theta=loaded)=0.6$$
	$$f(\theta|x)=\frac{f(x|\theta)f(\theta)}{\Sigma f(x|\theta)f(\theta)} \theta=\{loaded,fair\}$$
	$$=\frac{\binom 5 x [(.5)^5(.4)I_{\theta=fair}+(.7)^x(.3)^{5-x}(.6)I_{\theta=loaded}]}{\binom 5 x [(.5)^5(.4)+(.7)^x(.3)^{5-x}(.6)]}$$
	$$f(\theta|x=2)=\frac{.0125I_{\theta=fair}+.0079I_{\theta=loaded}}{.0125+.0079}$$
	$$=.612I_{\theta=fair}+.388I_{\theta=loaded}$$
	Now, <b>we can get the prbobaility of each case which cannot be calculated in the frequentist paradigm</b>. Because this calculation is using prior probability and make result including observation, we can say this process is updating our prior. Of course, If we change the prior, the result will be change. 
</p>
<p>
	<b>The bayesian approach is inherently subjective.</b>. It represents our perspective. But, it is totally mathematical result and we can get the proper result which is different from frequentist paradigm. <b>Although Frequentist paradigm hides the assumption of reference and tried to be objective, it cannot get good result. But, Bayesian start with subjective and get proper result in many cases</b>
</p>