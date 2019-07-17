---
title: 2D hist(Using Matplotlib)
subtitle:   "Let's make a 2D histogram"
layout: post
comments: true
author:     "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [Data Analysis, python]
---

<p>
&nbsp;&nbsp;&nbsp;When we need to plot data which have two variables like image matrix, the 2D histogram can be good solution. It has several option that can make people to show the behavior of data more easier. In this post, I'll talk about how to draw 2D histogram with <b>matplotlib</b> which is the module for <b>python</b>
</p>
### Implementation
- Simple way
<pre><code class="language-python line-numbers"  numbering>import numpy as np
import matplotlib.pyplot as plt
# Set matrix
x = np.arange(51).reshape(51,1)
y = np.arange(51).reshape(51,1)
z = x.T + y # Need to be 2d matrix
plt.imshow(z)
plt.show()
</code></pre>
<img src="/img/sum_x_y_fig.png" alt="x+y">
- Add axis name
<pre><code class="language-python line-numbers"  numbering>plt.xlabel('x')
plt.ylabel('y')
plt.title('x+y')
</code></pre>
- Add color bar
<pre><code class="language-python line-numbers"  numbering>fig, ax = plt.subplots()
cax = plt.imshow(z)
cbar = fig.colorbar(cax) # Set colorbar
</code></pre>
- Invert axis value
<pre><code class="language-python line-numbers"  numbering>plt.gca().invert_yaxis() 
</code></pre>
<img src="/img/sum_x_y_fig_with_option.png" alt="x+y with option">

<p>
&nbsp;&nbsp;&nbsp;It is also possible to show image data like picture which have RGB data. The way to implement is same with previous example. What we need to do is just make matrix and set matrix as a parameter of imshow
</p>
- Show image
<pre><code class="language-python line-numbers"  numbering># Use 2D image matrix which have RGB in each pixel
plt.imshow(image_matrix) 
</code></pre>
<center><img src="/img/plot1_in_one_canvas.png" alt="hand"></center>
- Split image by each channel(R/G/B)
<pre><code class="language-python line-numbers"  numbering>f, ax = plt.subplots(1,3) 
ax[0].imshow(image_matrix[0,:,:,0])
ax[1].imshow(image_matrix[0,:,:,1])
ax[2].imshow(image_matrix[0,:,:,2])
</code></pre>
<center><img src="/img/plot3_in_one_canvas.png" alt="3hand"></center>