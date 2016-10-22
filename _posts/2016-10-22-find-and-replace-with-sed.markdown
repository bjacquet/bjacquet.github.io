---
layout: post
title:  "Find & Replace in multiple files"
date:   2016-10-22 22:46:00 +0200
categories: sed shell
---
Today I wanted to change all my `.emacs.d/` files in one go. I've previously
prefixed my functions with `bj/` but find that too hard to type fast. `bj:`
seems to be an easier alternative. Not wanting to manually edit all those files
I looked into `grep` and `sed` to help me out!

First step: what are the files that have `bj/` in it?

{% highlight shell %}
$ grep "bj/" * -Rl
{% endhighlight %}

Second step: replace the file contents which match `bj/` with `bj:`

{% highlight shell %}
$ sed -i 's/bj\//bj:/g' file.el
{% endhighlight %}

`-i` makes `file.el` to serve as output as well.

Third step: putting it all together.

{% highlight shell %}
$ for f in `grep "bj/" * -Rl`; do sed -i 's/bj\/bj:/g' $f; done
{% endhighlight %}

Nice work!

PS. It took me about the same amount of time to write this post and to commit
[this][1] change.

[1]: https://github.com/bjacquet/dotemacs/commit/69a82032ce7a54f7834c17a1ad403e2bc158f7f7
