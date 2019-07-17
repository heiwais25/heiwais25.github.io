---
title: Classification 3(Multi class)
layout: post
author:     "JongHyun"
header-img: "img/machine_learning_backboard.jpg"
categories: [Data Analysis, Machine Learning]
---
<p>
	<span style="display:inline-block; width: 30px;"></span>In the classification, there are binary class classification and multi class classification. I already dealt with binary calss classfication <a href="/dataanalysis/machinelearning/2017/09/25/classification/">before.</a> Now let's talk about multi class classification. <b>Simply, multi class classification is used when there are discrete output and the number of output is larger than 2.</b> Multi class classification can be found easily in the world. For example, email-folding / tagging, medical diagram, wether forecasting and so on. The simplest multi class case would be 3 class case like below plot.
</p>
<img src="/img/octave/classification_example_multi_class.png" alt="classification_example">
<p>
	<span style="display:inline-block; width: 30px;"></span>We can see there are 3 main part which can be divided into each similar region. <b>Then, our queation would be how we can divide them into 3 region or find decision boundary based on our knowledge of binary class.</b> The answer is "Yes". We can use the similar method to multi class classification. The name of method is <b>one-vs-all.</b> It trains a logistic regression classifier(hypothesis) for each class to predict the probability. If we get new input after training, we can make prediction by picking the class that maximize hypothesis.
	$$h_\theta^{(i)}(x)=P(y=i|x;\theta)\text{      i : each class}$$
	$$\text{With new input x, choose      }\;\max_{\rm i} h_\theta^{(i)}(x)$$
</p>
