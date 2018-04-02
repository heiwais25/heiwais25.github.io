---
title: Overfitting
layout: post
author:     "JongHyun"
header-img: "img/machine_learning_backboard.jpg"
categories: [dataAnalysis, machineLearning]
---
### Overfitting
<p>
    <span style="display:inline-block; width: 30px;"></span><b>Overfitting</b> is one of the main difficulties in machin learning. It shows that many training sets or many features are not always the solution. If we get too many features, we might be able to fit our training sets to hypothesis well. However, there are also possibility that fails to new example. So, overfitting means when the hypothesis becomes useless although we have many features and fit well. Fortunately, there are some solution which can decrease the effect of overfitting.
</p>

> Machine learning is kind of fight that trying to find equilibrium between **under fit(less features, low order, High bias)** and **over fit(too many features, high order, high variance : due to higher order)**

### Regularization
<p>
    <span style="display:inline-block; width: 30px;"></span>The solution are <b>reducing the number of features</b> or <b>regularization.</b> The first method is trying to select manually which features need to be kept and which features need to be thrown out. Because the overfitting is due to so many features, it could be good idea. <b>Model selection algorithm</b> is one of algorithm which is based on this idea. But, it is possible to throw away some information which can be important. <br/>
    <span style="display:inline-block; width: 30px;"></span>Second method called <b>regularization</b> is keeping all features, but reducing magnitude or weight of each parameter theta. It works well when we have a lot of features which has each information. 
    $$\text{Suppose we have high order hypothesis }h_\theta(x) = \theta_0 + \theta_1x + \theta_2x^2 + \theta_3x^3$$
    $$\text{And want to decrease the effect of }\theta_3\text{ Then, we can write like this}$$
    $$\text{Minimizing cost function }\rightarrow \min[\Sigma^m_{i=1}\frac{1}{2m}(h_\theta(x^{(i)})-y^{(i)})^2 + 1000\theta_3^2]$$
</p>
> There are additional term theta 3 multiplied by 1000. It is uesed to decrease the effect of theta 3. To minimize total cost function, gradient descent will go to small theta 3 value. This kind of process is called **regularization.**

### Application of regularization
<p>
    <span style="display:inline-block; width: 30px;"></span>As we can see in the previous new cost function, we can decrease the effect of certain parameter that we don't want to include. We can also make generalized equation.
    $$J(\theta) = \frac{1}{2m}[\Sigma^m_{i=1}(h_\theta(x^{(i)})-y^{(i)})^2 + \lambda\Sigma^n_{i=1}\theta_j ^2]$$
    $$\text{Regularization parameter : }\lambda$$
    The first term is same with previous cost function. The second term is application of regularization. The reason why we don't include theta 0 is it behaves like offset or standard. By adjusting lambda value, we can decrease or increase how much each parameter affect to hypothesis. For example, if we increase lambda too large, the hypothesis will become like a horizontal line.
</p>
<p>
    <span style="display:inline-block; width: 30px;"></span>To find parameter minimizing the cost function, we can use same gradient descent. The only different thing is differentiation of lambda term. Because lambda only cotributes to theta except 0, we will get the different result of differentiation.(theta 0 and others)
    $$\text{Repect}\{$$
    $$\theta_0 = \theta_0 - \alpha\frac{1}{m}\Sigma^m_{i=1}(h_\theta(x^{(i)}-y^{(i)})x_0 ^{(i)}$$
    $$\theta_j = \theta_j - \alpha[\frac{1}{m}\Sigma^m_{i=1}(h_\theta(x^{(i)}-y^{(i)})x_j ^{(i)} + \frac{\lambda}{m}\theta_j]$$
    $$\}$$
</p>



