---
title: Normal equation
layout: post
author:     "JongHyun"
header-img: "img/machine_learning_backboard.jpg"
categories: [Data Analysis, Machine Learning]
---
<p>
	We studied <b>gradient descent</b> before. It is aiming at finding the feature value which make local minimum of cost function by repeating substitution of features and differentiation of cost function. There are also another method to find proper parameter of hypothesis which is called <b>normal equation method.</b>
</p>
### Normal equation
<p>
	<b>The princple of normal equaiton is solving for parameter value analytically.</b> To do this, we will utilize matrix representation. Suppose that we have 4 data set(= training example) of house price y which have 4 features(house size x1, number of bedrooms x2, number of floors x3 and age of home x4). It can written as a table.
	$$\begin{array}{c|c}
	x_0 & x_1 & x_2 & x_3 & x_4 & y \cr
	1 & 2105 & 5 & 1 & 45 & 460 \cr
	1 & 1416 & 3 & 2 & 40 & 232 \cr
	1 & 1534 & 3 & 2 & 30 & 315 \cr
	1 & 852 & 2 & 1 & 37 & 178 \cr
	\end{array}$$
	Let's represent it as a matrix including every value of feature.
	$$X = \left\lbrack\begin{matrix}
	1 & 2105 & 5 & 1 & 45 \cr
	1 & 1416 & 3 & 2 & 40 \cr
	1 & 1534 & 3 & 2 & 30 \cr
	1 & 852 & 2 & 1 & 37 \cr
	\end{matrix}\right\rbrack\text{           : }m\times (n+1) \text{Matrix} = 4\times (5+1) \text{Matrix}$$
	$$Y = \left\lbrack\begin{matrix}
	460 \cr
	232 \cr
	315 \cr
	170 \cr
	\end{matrix}\right\rbrack\text{           : } m = 4 \text{dimensional vector}$$
	$$\theta = \left\lbrack\begin{matrix}
	\theta_0 \cr
	\theta_1 \cr
	\theta_2 \cr
	\theta_3 \cr
	\theta_4 \cr
	\end{matrix}\right\rbrack\text{           : }n+1 = 5\text{ dimensional vector}$$
	$$\text{(m, n) }=\text{(number of training example, number of features)}$$	
</p>
<p>	
	Then, we can write this equation referring to relation of input value, feature and output value.
	$$X\theta = Y$$
	$$X^T X\theta = X^T Y$$
	$$\theta = (X^T X)^{-1} X^T Y$$
	The transformation matrix is used to calculate inverse matrix. We can expand this equation to more general case. <b>The one property of normal equation is that it doesn't need feature scaling which is used in gradient descent because it calculates analytically.</b> 
</p>
<p>	
	<b>There are some advantages of normal equation comparing to gradient descent. It doen't need to choose learning rate and iterate. By contrast, its disadvantage is that it needs to compute inverse matrix of feature matrix and it will be slow if n(number of features) is very large.</b> We need to consider which method will be better at each situation. Normally, if n is too large like 1000, we use gradient descent. If n is small, it will be better to use normal equation.
</p>
### Non-invertibility
<p>	
	You may wonder if the feature matrix is not invertible. Of course there are some case that matrix is not invertible. To use normal equation more in detail, we need to know when non-invertible matrix appear. 
</p>
<p>	
	We can divide into two cases. <b>One is using rebundant feature(linearly dependent).</b> In the case of rebundant feature, it is related to each other. For example, if the one feature is square of another feature, there are no meaning to calculate both. <b>The other is that if matrix has too many feature more than number of training sample.</b> In that case, we need to delete some features or use regularization. If we have struggle in non-invertibility, we need to consider these issue and do right way. Fortuantely, there are no calculation problem even though the matrix is non-invertible if we use pinv function in octave.
</p>
