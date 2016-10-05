---
layout: post
title:  "Layout works!"
date:   2016-10-05 12:46:00 +0200 
categories: jekyll update
---
Finally I got the layout working. The problem being an incorrect setting of `url:` on the
`_config.yml` file:

{% highlight shell %}
url: "http://bjacquet.github.io/" # the base hostname & protocol for your site
{% endhighlight %}

That forward slash at the end was causing an incorrect path for the css files and all other `hrefs`. Only when I looked at the html source code that I notice the horror!

{% highlight html %}
<link rel="stylesheet" href="http://bjacquet.github.com//css/main.css">
{% endhighlight %}
