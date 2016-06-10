---
layout: post
title:  "JavaScript Scope"
date:   2016-6-9
---

<p class="intro"><span class="dropcap">I</span> recently encountered an advanced raw JavaScript problem involving scope that caught me off guard.  I thought to myself this is something that I need to investigate, get to the bottom of and better understand.  So I set out to do just that.  I recreated the problem and found as many ways to solve it as I could.  Now to some of you advanced JavaScript'ers, this is probably a no brainer.  However, I felt as if I had in depth JavaScript knowledge and apparantly I was mistaken.  So here we go.</p>

Here's the code that tripped me up:

{% highlight javascript %}
var user = {
  index: 0,
  clickHandler: function(event) {
    this.index++;
    console.log('index is ', this.index);
  }
};

var button = document.getElementById('btnTest');
button.addEventListener('click', user.clickHandler, false);
{% endhighlight %}

The question was, "What is the value of this.index inside of the clickHandler in the console.log statement?".

At a first quick glance it appeared to be 1.  However looking it over a bit more, realizing that clickHandler was an anonymous function, `this` looked like it might be `window` so the value of `index` was `undefined`.  Well the answer to this question is that this.index is actually `NaN`.  `this` was actually the DOM object of the button.  Since the button did not have an attribute with the name of index, when we try to add 1 to undefined we get that value of `NaN`.  So therefore we have to tell clickHandler to take on a new `this`.  So far I have discovered 3 different ways of handling this.

The first way is to change `this` to `user` like so:

{% highlight javascript %}
var user = {
  index: 0,
  clickHandler: function(event) {
    user.index++;
    console.log('index is ', user.index);
  }
};

var button = document.getElementById('btnTest');
button.addEventListener('click', user.clickHandler, false);
{% endhighlight %}


This uses the concept where the name of the object is in scope to all local properties and functions.