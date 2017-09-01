---
title: 2d histogram(Using Matplotlib)
subtitle:   "Let's make a 2D histogram"
layout: post
author:     "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [dataAnalysis, python]
---

<p>
&nbsp;&nbsp;&nbsp;When we need to plot data which have two variables, the 2D histogram can be good solution. It has several option 
That can make people to show the behavior of data more easier. In this post, I'll talk about how to draw 2D histogram 
with <b>matplotlib</b> which is the module for <b>python</b>
</p>
<p>
&nbsp;&nbsp;&nbsp;Let's set variable simplified version of x and y using <b>numpy</b> library. And make array which take data in the 
matrix form(51x51). In this case, I set <code>z = x + y</code>.
<pre><code>import numpy as np
x = np.linspace(0, 50, 51)
y = np.linspace(0, 50, 51)
z = np.array([x_val + y_val for x_val in x for y_val in y]).reshape(51,51)
</code></pre>
</p>
<p>
	&nbsp;&nbsp;&nbsp;Now we finished the preparation for making 2D histogram (z as a function of x and y). It's time to make canvas using matplotlib and
	set z matrix value in the canvas using <b>imshow</b> option. If we run this code, we can see the figure like the below one.
</p>
<pre><code>import matplotlib.pyplot as plt
fig, ax = plt.subplots()
cax = plt.imshow(z)
plt.show()
</code></pre>
<img src="/img/sum_x_y_fig.png" alt="x+y">
<p>
	&nbsp;&nbsp;&nbsp;If you could reproduce previous figure, it's almost done! After adding information of plot in the figure, we can see
	last figure like this. Now, you can change variable x, y and function z of x, y to get your own 2D histogram
</p>
<pre><code>fig.subplots_adjust(right=0.97, left=0.15) # It adjust the position of plot in the canvas
cbar = fig.colorbar(cax) # Set colorbar
plt.gca().invert_yaxis() # Set the order of y_axis start from 0
plt.title('x+y')
plt.xlabel('x')
plt.ylabel('y')
</code></pre>
<img src="/img/sum_x_y_fig_with_option.png" alt="x+y with option">
