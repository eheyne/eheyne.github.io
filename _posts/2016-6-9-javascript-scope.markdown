---
layout: post
title:  "Post With A Code Snippet"
date:   2016-6-9
---

<p class="intro"><span class="dropcap">I</span> recently encountered an advanced raw JavaScript problem involving scope that caught me off guard.  I thought to myself this is something that I need to investigate, get to the bottom of and better understand.  So I set out to do just that.  I recreated the problem and found as many ways to solve it as I could.  Now to some of you advanced JavaScript'ers, this is probably a no brainer.  However, I felt as if I had in depth JavaScript knowledge and apparantly I was mistaken.  So here we go.</p>

Here's the code that tripped me up:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll's GitHub repo][jekyll-gh].

[jekyll-gh]: https://github.com/mojombo/jekyll
[jekyll]:    http://jekyllrb.com
