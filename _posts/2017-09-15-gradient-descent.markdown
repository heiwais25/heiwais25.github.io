---
title: Gradient descent
layout: post
author:     "JongHyun"
header-img: "img/machine_learning_backboard.jpg"
categories: [dataAnalysis, machineLearning]
---
### Gradient descent
<p>
	<b>Gradient descent is nice technique which can be used to find parameter of hypothesis.</b> This method can be used for not only linear regression but also other algorithm. It is used when we have function and want to minimize it. It means we can use this method in many general cases. Briefly, gradient descent is consist of two steps. Suppose that we have hypothesis which have two parameter like linear regression. Then, the steps can be written like this
	$$\text{1. Start with some } \theta_0, \theta_1$$
	$$\text{2. Keep changing } \theta_0, \theta_1 \text{to reduce } J(\theta_0, \theta_1) \text{ until we find a minimum value}$$
	In this process, <b>we need to compare cost function value with neighborhood value every time and find way to decrease its value. Then, it will arrive in local minimum which we want.</b> Gradient descent algorithm can be written like this.
	$$\begin{gather}\text{repeat until convergence}\{\\ \theta_j := \theta_j -\alpha \frac{\partial }{\partial \theta_j}J(\theta_0, \theta_1)\;\text{for }j=0,1\\ \}\text{simultaneously update }\theta_0 \text{ and } \theta_1 \end{gather}$$
	$$\text{learning rate : }\alpha$$
	learning rate means how big a step we take at each step with creating descent. If it is large, there will be huge step. If it is small, there will be small step
</p>
<p>
	<b>Depending on the size of learning rate, the accuracy and time cost will be different.</b> If it is too small, it will take so much time although we can get better accuracy result. If it is too large, it might be fail to converge or even diverge although it took short time. 
</p>
<p>
	When the cost function value is located at local minimum, the process of gradient descent will stop. So, <b>we don't need to change learning rate while we run the gradient descent</b> because it will take smaller step as it approaches to local minimum
</p>
### With Linear Regression(Single feature)
<p>
    Let's apply the concept of gradient descent to linear regression more in detail. <b>We can make partial deriavative more simple in the case of linear regression because it is simple.</b> Maybe it would be different if the equation is quadratic or cubic form.  
    $$\frac{\partial}{\partial \theta_j}J(\theta_0, \theta_1) =  \frac{\partial}{\partial \theta_j} \frac{1}{2m}\Sigma^m_{i=1}(h_\theta (x^{(i)})-y^{(i)})^2$$
    $$ = \frac{\partial}{\partial \theta_j}\frac{1}{2m}\Sigma^m_{i=1}(\theta_0+\theta_1 x^{(i)}-y^{(i)})^2$$
    $$= \frac{1}{m} \Sigma^m_{i=1}(h_\theta(x^{(i)})-y^{(i)})+\frac{1}{m}\Sigma^m_{i=1}(h_\theta(x^{(i)})-y^{(i)})x^{(i)}$$
    $$\text{If we set }\theta_0 = \theta_0 \cdot x^{(i)} _0, \theta_1 \cdot x^{(i)}= \theta_1 \cdot x^{(i)} _1$$
    $$\text{Then, }\frac{\partial}{\partial \theta_j}J(\theta) = \frac{1}{m}\Sigma^m_{i=1}(h_\theta(x^{(i)}) - y^{(i)}) x^{(i)} _j$$
</p>
<p>
	This kind of process which is using all training sample is called <b>Batch gradient descent.</b>. By contrast, using subset of training sample also exist.
</p>
### With Linear Regression(Multiple features) 
<p>
	We have dealt with linear regression of single feature. Then, how we can apply the linear regression of multiple features? Fortunately, we can use linear regression in the same way with single feature case. <b>What we need to do is combining feature by linear combination like this equation.</b>
	$$\text{Single feature : }h_\theta(x) = \theta_0 + \theta_1 x$$
	$$\text{Multiple n feature : }h_\theta(x) = \theta_0 + \theta_1 x_0 + \cdots + \theta_n x_n$$
	If we apply new meaningless feature(= 1) to the equation, we can also use matrix form.
	$$\text{Multiple n feature : }h_\theta(x) = \theta_0 x_0+ \theta_1 x_0 + \cdots + \theta_n x_n$$
	$$x = \left\lbrack\matrix{ x_0 \cr x_1 \cr \vdots \cr x_n} \right\rbrack \in R^{n+1}\text{       }\theta = \left\lbrack\matrix{ \theta_0 \cr \theta_1 \cr \vdots \cr \theta_n} \right\rbrack \in R^{n+1}$$ 
	$$\text{Matrix form : }h_\theta(x) = \theta^T x$$
	As we can see, the equation becomes much simpler although we are using multiple features. <b>This kind of calculation using matrix form not only makes representation simpler but also calculation becomes faster in Octave or Matlab.</b>
</p>




