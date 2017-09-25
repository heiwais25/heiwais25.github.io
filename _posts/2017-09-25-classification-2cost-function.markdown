---
title: Classification 2(Cost function)
layout: post
author:     "JongHyun"
header-img: "img/machine_learning_backboard.jpg"
categories: [dataAnalysis, machineLearning]
---
<p>
	<span style="display:inline-block; width: 30px;"></span>We have already dealt with cost function in the linear regression. It's objective is measuring how mcuh different hypthesis with training set. In linear regression, the cost function was the form of square of difference. It works well in linear regression. However, logistic regression or classification case can not use linear regression because it has many local minimum and it is hard to find global minimum. In other word, square of difference works well only when the cost function shows "convex" form which have only one global minimum. So, we need new cost function which is suit for logistic regression. It needs to represent the characteristics of logistic regression.
	$$\text{Cost Function}(h_\theta(x),y) = {-log(h_\theta(x)) \text{ if }y=1\atopwithdelims [ ] {-log(1-h_\theta(x))}\text{ if }y=0}$$
	It represent the cost goes infinity if hypothesis is 0 when training set is 1 which can not be admitted. Opposite case is same. If we write the cost function in one equation, it looks like this
	$$\text{Cost Function}(h_\theta(x),y) = -ylog h_\theta(x) - (1-y) log(1 - h_\theta(x))$$
	$$J(\theta) = \frac{1}{m}\Sigma_{i=1}^m (-y^{(i)}log h_\theta (x^{(i)}) - (1-y^{(i)})log(1-h_\theta(x^{(i)})))$$
</p>
<p>
	<span style="display:inline-block; width: 30px;"></span>To find the proper parameter of hypothesis, we need to find parameter which is minimizing the cost function. The good way to find easily is <b>gradient descent.</b>. The shape of gradient descent is same with linear regression.
	$$\text{Repeat }\{$$
	$$\theta_j = \theta_j - \alpha \frac{\partial}{\partial \theta_j}J(\theta)$$
	$$\}$$
</p>
