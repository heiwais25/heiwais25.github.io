---
title: Classification 2(Cost function)
layout: post
author:     "JongHyun"
header-img: "img/machine_learning_backboard.jpg"
categories: [Data Analysis, Machine Learning]
---
<p>
	<span style="display:inline-block; width: 30px;"></span>We have already dealt with cost function in the linear regression. It's objective is measuring how mcuh different hypthesis with training set. In linear regression, the cost function was the form of square of difference. It works well in linear regression. However, logistic regression or classification case can not use linear regression because it has many local minimum and it is hard to find global minimum. In other word, square of difference works well only when the cost function shows "convex" form which have only one global minimum. So, we need new cost function which is suit for logistic regression. It needs to represent the characteristics of logistic regression.
	$$\text{Cost Function}(h_\theta(x),y) = {-\log(h_\theta(x)) \text{ if }y=1\atopwithdelims [ ] {-\log(1-h_\theta(x))}\text{ if }y=0}$$
	It represent the cost goes infinity if hypothesis is 0 when training set is 1 which can not be admitted. Opposite case is same. If we write the cost function in one equation, it looks like this
	$$\text{Cost Function}(h_\theta(x),y) = -y\log h_\theta(x) - (1-y) \log(1 - h_\theta(x))$$
	$$J(\theta) = \frac{1}{m}\Sigma_{i=1}^m (-y^{(i)}\log h_\theta (x^{(i)}) - (1-y^{(i)})\log(1-h_\theta(x^{(i)})))$$
</p>
<p>
	<span style="display:inline-block; width: 30px;"></span>To find the proper parameter of hypothesis, we need to find parameter which is minimizing the cost function. The good way to find easily is <b>gradient descent.</b>. The shape of gradient descent is same with linear regression. The only different thing comparing to linear regression is hypothesis. So, it will be good if we use vector and feature scaling.
	$$\begin{gather}\text{Repeat }\{\\\theta_j = \theta_j - \alpha \frac{\partial}{\partial \theta_j}J(\theta)\\ \}\end{gather}$$
</p>
### Advanced optimization
<p>
	<span style="display:inline-block; width: 30px;"></span>It is important not only learning the theory, but also how to apply them in the code. Basically, we will write gradient descent code and run them continuously to find local minimum. Because we always want to make it faster, there are another optimization algorithms. These are <b>1. Conjugate gradient, BFGS, L-BFGS.</b> The advantages of these algorithms are it doesn't need to manually pick learning rate which is required to gradient descent. However, the disadvantage is it is more complex to apply the algorithm. The good thing is we can use library! There are many libararies which can make calculation faster.
</p>
<p>
	<span style="display:inline-block; width: 30px;"></span>For example, there are <code>fminunc</code> function (f min under constraint) in <b>octave</b>. It can be used by setting how many we will iterate and which costfunction we will use. By using this kind of function, we can decrease our cost time.
</p>