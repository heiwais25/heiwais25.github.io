---
title: Basic concept of supervised learning
layout: post
author:     "JongHyun"
header-img: "img/machine_learning_backboard.jpg"
categories: [dataAnalysis, machineLearning, octave]
---
### What is supervised learning?
<p>
	<b>Supervised learning is used when we know the right answer or data sets are can be fully catergorized.</b> For example, When we have data of height and age of people, we might can find relation between age and height. Let's assume we found the relation between age and height fortunately. Then, we can predict height with certain age value. This kind of process is called supervised learning. <b>If we have uncategorized or unlabeld data, we need to use unsupervised learning.</b>
</p>
### Model representation
<p>
	When we have data sets from various field, what we want to do is predict new data based on existed data. For example, let's consider scattered distribution like below plot.
</p>
<img src="/img/octave/linear_refression.png" alt="linear_regression">
<p>
	To understand its behavior or the relation between x and y, we can use modeling. The simplest way to modeling this kind of data is fitting to line. Let's draw line which looks fit well. 
</p>
<img src="/img/octave/linear_regression_with_line.png" alt="line_reg_line">
<p>
	Through this kind of method, we can achieve our purpose. This kind of algorithm is called <b>supervised learning algorithm</b> because it has expected right-answer to each data. In detail, it is also called <b>regression problem</b> because it predict real-valued output not discrete data.(If output is discrete-value, it is called <b>classification</b>).
</p>
### How it works?
<p>
	In supervised learning, there are main components of data set. there are <b>number of training example, input variable(feature), output variable(target variable).</b> If we represent the structure of supervised learning, it looks like this.
	$$\text{Training set} \rightarrow \text{Learning Algorithm} \rightarrow \text{output function h} $$
	output function is called hypothesis. <b>The main process in this learning is optimize our hypothesis by using training set and learning algorithm which can represent data sets well.</b> I think we can call it as learning process from data set.
</p>
<p>
	As we saw in the previous plot, the linear model looks fit well. If we use linear regression, the left thing is finding the equation of line or finding parameter of hypothesis. There are only two main variable in linear equation. This simplicity is one of main reason why linear regression is so popular.
</p>
<p>
	To find the proper parameter of hypothesis, we can use the concept of cost function. I'll deal with cost function later.
</p>
