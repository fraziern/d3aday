---
layout: post
title:  "Summer Heat"
date:   2019-12-10 06:38:38 -0500
tags: bar-graph small-multiples
thumbnail_path: /assets/img/summerheat_thumb.jpg
thumbnail_alttext: Summer heat
index: 03
---
In this exercise, we're building a deceptively simple bar graph. The inspiration comes from the clever "Amount of Love According To Bands" by mattmattmatt on [GraphJam](https://cheezburger.com/2511676160/amount-of-love-according-to-bands).

![Funny bar graph](/assets/img/love_inspiration.png)

My version is called "How Much Time Do We Need." Note the text-based axes, and the word wrap in the X-axis labels. (Note the second bar isn't exactly to scale... but I needed to make room on the Y-axis for all the labels so I fudged a bit.)

![Another funny bar graph](/assets/img/love_solution.jpg)

The complete data is below (JSON format). To make things easier for setting up a quick word wrap routine, I added a `/` where a line break should go, in the X-axis labels.

{%highlight json %}
[
  ["Hall and/Oates","Did It In A Minute",1],
  ["Solomon/Burke","A Minute to Rest and a Second To Pray", 3],
  ["Frank/Sinatra","5 Minutes More",5],
  ["Johnny/Cash","25 Minutes to Go",25],
  ["Scorpions","Every Minute Every Day",100]
]
{% endhighlight %}


My solution can be found on [codepen](https://codepen.io/fraziern/pen/bGGWwKZ)

### Standard D3aday Instructions

1. Build your version of the inspiration viz or my solution viz
1. Use only vanilla HTML, CSS, and JavaScript, as well as D3.js (I used v.5.0)
1. Feel free to use my data for purposes of the exercise

Good luck!