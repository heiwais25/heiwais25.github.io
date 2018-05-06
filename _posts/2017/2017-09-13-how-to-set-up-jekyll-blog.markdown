---
title: How to set up jekyll blog
layout: post
author:     "JongHyun"
header-img: "img/post-bg-06.jpg"
categories: [Tip]
---
<p>
	Let's make your own blog. It is easy to make blog when we use jekyll and github. <b>Github have service that support representing the information included in github repository.</b> This blog is also an example of github blog. Jekyll is tool which can make our static blog much easily. This package is based on <b>ruby</b>.
</p>
<p>
	<b>First of all,</b> you need to make your own repository in the github. It will be easy to make your repository in the github site. <b>The only important thing in this step is your repository name. You should make your repository name as <code>your_github_id.github.io</code>.</b> Then github will recognize it and represet this on the website(same address with your repository)
</p>
<p>
	<b>Second,</b> we need to setup jekyll. I'll assume you already install the ruby. Regardless of your OS, type this code into your terminal
<pre class="language-ruby line-numbers"><code>gem install jekyll bundler
jekyll new your_blog_name # Replace it with what you want for your blog name
cd your_blog_name 
jekyll serve	</code></pre>

	After typing <code>jekyll serve</code>, It will start to make your jekyll directory as a website. To see how it works, type <code>localhost:4000</code> in your browser. Then you can see your own blog!
</p>

> If you have some problem when you type <code>jekyll serve</code>, you can solve that problem by installing specific package which will be written on the terminal. 
> In my case, there was issue like <code>Could not find jekyll-paginate</code> and <code>Could not find jekyll-feed</code>. After installing by gem install, This kind of problem will disappear. 

<p>
	<b>Finally,</b> the only thing left is uploading your nice blog to github. 
	<pre class="language-git line-numbers"><code>git push	</code></pre>
	After this command, you can see your website which have same address with your repository. 
</p>