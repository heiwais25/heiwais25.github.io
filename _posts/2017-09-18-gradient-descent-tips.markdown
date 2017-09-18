---
title: Gradient Descent Tips
layout: post
author:     "JongHyun"
header-img: "img/machine_learning_backboard.jpg"
categories: [dataAnalysis, machineLearning]
---
<p>
	If you use linear regressions of <a href="/dataanalysis/machinelearning/octave/2017/09/15/gradient-descent/">multiple features</a>, you can use these tips to decrease time required to find local minimum. The main point is simple. We need to make scale of features similar with each other by using some method. One is <b>Feature scaling.</b> The other is <b>Mean normalization.</b> Of course we can use both together.
</p>
### Feature scaling
<p>
	The idea of feature scaling is <b>making sure that features are on similar scale.</b> Suppose that there are two features(A,B). The range of A is 0 ~ 1000 and B is 1 ~ 5. Then, it will take so much time to find local minimum because there range is so different to each other. If we draw cost function of two feature in 2D, we can see ellipse which have thin major axis. 
</p>
<p>
	To decrease required time, we can make range of A and B similar. We prefer smaller range than larger range. We can modify each parameter like this by dividing some value. It would be better to use maximum value in training sample.
	$$A' = \frac{A}{1000}$$ 
	$$B' = \frac{B}{5}$$
	Then, the range of each parameter will be 0 ~ 1. It will decrease required time. So, getting every feature into approximately -1 ~ 1 range is called mean normalization.
</p>
### Mean normalization 
<p>
	The other method is subtracting mean value to every training example data. Through this method, we can make the mean value to 0. That is why it is called mean normalization. For example, if we have dataset A = [1, 3, 5, 7, 9] which is 1,3,5,7,9, the mean value is 5. Then, we need to subtract 5 to eachvalue of dataset. It can be written like this.
	$$A' = A-5 = [-4, -2, 0, 2, 4]$$
</p>
<p>
	We can use feature scaling and mean normalization together. It can be written like this.
	$$X' = \frac{X-\mu}{\sigma} = \frac{\text{data value} - \text{mean value}}{\text{Range : (Max - Min) or Standard deviation}}$$
</p>
