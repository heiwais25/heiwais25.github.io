---
title: Predictive interval 2
layout: post
author:     "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [Data Analysis, statistics]
---
This is continuous post to <a href="/dataanalysis/statistics/2017/09/05/predictive-interval-1/">Predictive interval 1</a>
<p>
	<b>How the posterior predictive distribution will change after getting actual data?</b> For example, if we flip coin one time and observe one head event, what is the predictive distribution for second flip? We can write the posterior predictive distribution after observing one event. The important point is we use posterior distribution instead of using prior distribution.
	$$f(y_2|y_1)=\int f(y_2|\theta,y_1)f(\theta|y_1)d\theta \;\text{if} \;Y_2\bot Y_1,\,\int f(y_2|\theta)f(\theta|y_1)d\theta$$
	$$f(y_2|Y_1=1)=\int^1_0 \theta^{y_2}(1-\theta)^{1-y+2}2\theta d\theta$$
	$$= \int^1_0 2\theta^{y_2+1}(1-\theta)^{1-y_2}d\theta\;\;y_2=0\,or1$$
	$$f(y_2|Y_1=1) = \cases{
		\int^1_0 2\theta(1-\theta) d\theta = \frac{1}{3} & \text{if}\;Y_2=0 \cr
		\int^1_0 2\theta^2 d\theta = \frac{2}{3} & \text{if}\;Y_2=1
		}$$
</p>
<p>
	We can see there are difference in predictive distribution because we observe one head. Through this kind of approach, we can update our posterior from prior with a number of data.
</p>