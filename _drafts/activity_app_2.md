---
layout: post
title:  "Activity App Part 1"
date:   2020-08-16 06:38:38 -0500
tags: shapes arc bar-chart
thumbnail_path: /assets/img/activity-pt1_solution_B.jpg
thumbnail_alttext: Activity App Part 1
index: 15
---

Three Parter

challenges for first:
  - thinking in arcs
  - using scaleBand in a different way, but in some ways the same
  - icons

challenges for second:
  - organizing data
  - turning the arc generator into a function
  - finding the week number

The following exercise is the first in a three-part series, where we'll be working on recreating an icon of modern data visualization - the Apple Activity rings.

  - Part 1 - The Day Display
  - Part 2 - The Calendar Display
  - Part 3 - Adding Interactivity

### The Inspiration

The inspiration for this series is the iPhone and Apple Watch Activity app. More specifically, our focus will be on the activity rings that display for each day or month. For those unfamiliar, the Activity app is Apple's "quantified self" interface for exploring your health activity. It's intimately tied to the Apple Watch - as you wear the watch throughout the day, it keeps track of 3 different kinds of activities. As Apple's *[Close Your Rings](https://www.apple.com/watch/close-your-rings/)* site says, "Three rings: Move, Exercise, Stand. One goal: Close them every day."

![Apple Activity Rings](/assets/img/activity-pt1_inspiration.jpg)

The three rings represent percentage to a goal - circular progress bars if you will. If they're "closed," you've gotten to 100% of that goal. Each day you get 3 new rings, 3 new goals.

From a dataviz point of view, this is an example of a *[radial bar chart.](https://datavizcatalogue.com/methods/radial_bar_chart.html)* [Several commenters](https://www.visualisingdata.com/2017/09/problems-barc-charts/) on the web have criticized this type of viz, [saying]((https://www.data-to-viz.com/caveat/circular_barplot_accordeon.html)) for example that "a bar on the outside will be longer ... than one on the inside, even with an equal value." While true, this seems to be less of an issue when the bars represent percentages. (This format also, unlike standard bar charts, translates well when used in large numbers of multiples. We'll explore this further in Part 2 of the series.)

So let's build our own with D3.

### The Exercise

The data we're going to use

starting simply - will use arcs
[d3-shape](https://github.com/d3/d3-shape)

{%highlight JavaScript %}
function isDecade(date) {
  return date.getUTCFullYear() % 10 === 0;
}
{% endhighlight %}

to determine the necessary radii, recommend a band scale

in some ways this is simpler than most viz's - there's no Y scale, and no axes.

### A Basic Solution

My basic result looks like so:

![Activity App Solution A](/assets/img/activity-pt1_solution_A.jpg)

My JavaScript code for the basic solution is below, but I encourage you to try it yourself. 

{% highlight JavaScript %}

{% endhighlight %}

### Extra Credit

![Activity App Solution A](/assets/img/activity-pt1_solution_B.jpg)

- icons
- background

[Open Iconic](https://useiconic.com/open)

Complete code and a working example are available on [codepen](https://codepen.io/fraziern/pen/LYEePWe).
