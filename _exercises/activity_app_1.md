---
layout: post
title:  "Activity App Part 1"
date:   2020-08-16 06:38:38 -0500
tags: shapes arc bar-chart
thumbnail_path: /assets/img/activity-pt1_solution_B.jpg
thumbnail_alttext: Activity App Part 1
index: 15
---
Welcome to the first in a three-part series, where we'll be working on recreating an icon of modern data visualization - the Apple Activity rings.

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

The data we're going to use for Part 1 is simple:

`[
  {value: 20},
  {value: 30},
  {value: 100}
]`

This data emulates the percentage-completion for 3 categories. The task is to display these categories as concentric rings.

To do this in d3, you should make use of the "arc" methods in [d3-shape](https://github.com/d3/d3-shape). These are more commonly used for donut and pie charts, but will do what we need to do here as well.

in some ways this is simpler than most visualizations - there's no Y scale, and no axes. There are some unique challenges here though. Perhaps the biggest one is just wrapping your head around working with arcs and circles instead of straight lines.

### A Basic Solution

My basic result looks like so:

![Activity App Solution A](/assets/img/activity-pt1_solution_A.jpg)

As simple as this visualization looks, there are a few steps involved in going from data to display here. The first is to define the arc generator. If you've used d3's line generator before, it's the same idea, but the attributes are different. For example:

{%highlight JavaScript %}
  const arc = d3.arc()
    .innerRadius(5)
    .outerRadius(10)
    .startAngle(0)
    .endAngle(Math.PI)
    .cornerRadius(5)
{% endhighlight %}

This generator uses constant values, and will output a "d" string that can be applied to a path. As with line generators you can use functions as well, which get passed data and indices, e.g.:

{%highlight JavaScript %}
  const arc = d3.arc()
    .innerRadius((d, i) => something)
    .outerRadius((d, i) => somethingElse)
    .startAngle(0)
    .endAngle(Math.PI)
    .cornerRadius(5)
{% endhighlight %}

Once the arc generator is defined, it can be used with paths that are bound to data. For example:

`path.datum(data).attr("d", arc);`

One additional preliminary step is not strictly necessary but will make the layout much easier to manage - using a scale. Although there are circles and arcs here, it's still essentially a bar graph, and we can use d3's scaleBand to do some of our calculations for us. In this case, we'll use it to find the radii we need:

{%highlight JavaScript %}
const r = d3.scaleBand()
    .domain(d3.range(data.length))
    .range([innerPad, outerRadius])
    .paddingInner(.1);
{% endhighlight %}

My JavaScript code for the basic solution is below, but I encourage you to try it yourself first.

{% highlight JavaScript %}
const outerHeight = 500;
const outerWidth = 500;
const margin = { top: 0, bottom: 00, left: 00, right: 00 };
const width = outerWidth - margin.left - margin.right;
const height = outerHeight - margin.top - margin.bottom;

// responsive margin convention
const chart = d3
  .select(".chart")
  .append("svg")
    .attr("viewBox", `0 0 ${outerWidth} ${outerHeight}`)
  .append("g")
    .attr("transform", `translate(${width / 2},${height / 2})`);

function createChart(data) {
  
  const bandColors = [
    "#03C2DD",
    "#A9FC00",
    "#F62063"
  ];
  
  const innerPad = 50;
  const outerRadius = 200;
  
  // band scale
  const r = d3.scaleBand()
            .domain(d3.range(data.length))
            .range([innerPad, outerRadius])
            .paddingInner(.1);
  
  const arcs = d3.arc()
    .innerRadius((d,i) => r(i))
    .outerRadius((d,i) => r(i) + r.bandwidth())
    .startAngle(0)
    .endAngle(d => (d.value / 100) * 2 * Math.PI)
    .cornerRadius(r.bandwidth() / 2);
  
  chart.append("g").selectAll("path")
    .data(data)
    .join("path")
    .attr("d", arcs)
    .attr("fill", (d,i) => bandColors[i]);
}

const data = [
  {value: 20},
  {value: 30},
  {value: 100}
]

createChart(data);
{% endhighlight %}

### Extra Credit

If you want to go beyond the most basic solution, you can try adding a couple of enhancements.

![Activity App Solution A](/assets/img/activity-pt1_solution_B.jpg)

The first is a set of icons, that help describe the meaning of each circle. I've used the icon font available at [Open Iconic](https://useiconic.com/open) for this.

Incorporating these into a D3 viz isn't entirely intuitive, but I've found success with the below method:

* Add the font CSS file to your project
* Open up the file and find the font name and Unicode characters you're looking for
* Using D3, add `text` elements to your SVG, with the `font-family` and characters you found in the CSS

NOTE: The CSS file might define the unicode with a `\e` - if so, change it to `\ue` in your code.

{% highlight JavaScript %}
  chart.selectAll("text")
    .data(["\ue037", "\ue02f", "\ue036"])
    .join("text")
      .attr("font-family", "Icons")
      .attr("font-size", "2em" )
      .attr("text-anchor", "middle")
      .text(d => d)
      .attr("transform", (d,i) => `translate(20,${-r(i) - 5})`)
{% endhighlight %}

In addition to icons, you can also add a background (like the inspiration viz has). This looks nifty and helps define the rings as progress bars.

Doing so is a matter of drawing out each bar at 100% first, with low opacity. Then drawing the actual graph on top of that:

{% highlight JavaScript %}
  const bgData = data.map(el => ({value: 100}));
  
  // create background first
  chart.append("g").selectAll("path")
    .data(bgData)
    .join("path")
    .attr("d", arcs)
    .attr("fill", (d,i) => bandColors[i])
    .attr("opacity", .2)
  
  // ... then overlay with the actual chart
{% endhighlight %}

Make sure you draw the 2 versions in their own `g` groups, so you're not overwriting paths.

Have fun and good luck if you're trying this on your own! Complete code and a working example are available on [codepen](https://codepen.io/fraziern/pen/KKzzNLj).
