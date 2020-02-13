---
layout: post
title:  "Slopegraph pt.2: Improved with Data Points"
date:   2019-12-10 06:38:38 -0500
tags: slopegraph selection parent-data
thumbnail_path: /assets/img/babynames_3_solution.jpg
thumbnail_alttext: Stack Exchange
index: 8
---
In the [first slopegraph exercise](% link _exercises/baby_names_2.md %), I created this visualization of top 10 female baby names.

![Baby Names Rankings](/assets/img/babynames_2_solution.jpg)

In sticking with my aim to keep the exercises short and quick, I left both a substantive issue and a style issue for a follow-up exercise. Substantively - the "Harper" point isn't shown for 2018, because we don't have data points added. That will be our first task in this exercise.

### Scatterplots and Accessing Parent Data

The idea is to overlay a scatterplot on top of the existing plot, which will add a point for Harper/2018, and will also make the data points clearer for the rest of the graph. I found some example code from Max Goldstein [here](http://bl.ocks.org/mgold/6a32cec6380b6ce75c1e) for doing something like this, which got me part of the way.

A key challenge with this is to keep track of what color each point should be. This requires accessing the data bound to the parent group object. Max solves it using "a little known third argument j" to index the group.

{% highlight JavaScript %}
city.selectAll("circle")
    .data(function(d){return d.values})
    .enter()
    .append("circle")
    .attr("r", 3)
    .attr("cx", function(d) { return x(d.date); })
    .attr("cy", function(d) { return y(d.temperature); })
    .style("fill", function(d,i,j) { return color(cities[j].name); });
{% endhighlight %}

However, this code is based on D3 v.3. In D3 v.4 the "j" was removed. See Bostock's discussion about indexing parent groups [on github](https://github.com/d3/d3-selection/issues/47).

Most modern solutions to this I've seen involve [complex workarounds](https://stackoverflow.com/questions/38233003/d3-js-v4-how-to-access-parent-groups-datum-index/382358). Some of these involve selection.each. As Bostock describes it on github: 
"...you can use selection.each and create a closure that exposes parent and child data simultaneously. "

But there is another option mentioned almost in passing, which I think is elegant in its simplicity and works well here:
"You can push that parent data down into the child data, replicating it for each child."

This is what I've done here.

Thus,
{% highlight JavaScript %}
  let data = names.map(name => {
    let arr = rawData.map(d => ({Year: d.Year, Rank: d[name]}));
    return {Name: name, Data: arr};
  })
{% endhighlight %}

becomes
{% highlight JavaScript %}
  let data = names.map(name => {
    let arr = rawData.map(d => ({Year: d.Year, Rank: d[name], Name: name}));
    return {Name: name, Data: arr};
  })
{% endhighlight %}

By tacking on the parent name to each data point during the data munging step, you no longer need to try and figure out how to access the parent later. The number of data points here is small too, so there's not really a memory or performance issue with doing so.

Once you have the parent name, you can easily get the color information.

### forEach vs. .data(data)

The second, perhaps less difficult, problem I wanted to solve is stylistic. My prior code makes use of "forEach" instead of the more idiomatic D3 data-bind. This is a fairly straightforward fix.

Old code:
{% highlight JavaScript %}
  data.forEach(name => {
    chart.append("path")
      .attr("class", "line")
      .attr("d", valueline(name.Data))
      .attr("stroke", colors(name.Name));
  });
{% endhighlight %}

New code:
{% highlight JavaScript %}
  var name = chart.selectAll(".name")
      .data(data)
    .join("g")
      .attr("class", "name");
  
  name.append("path")
      .attr("class", "line")
      .attr("d", d => valueline(d.Data))
      .style("stroke", d => color(d.Name));
{% endhighlight %}

The resulting graph looks like this.

![Baby Names Rankings](/assets/img/babynames_3_solution.jpg)

The complete HTML/CSS/JS code and working example are on [codepen](https://codepen.io/fraziern/pen/bGNzKoz).

The baby name data I used is [here](https://gist.githubusercontent.com/fraziern/6ca21ed36b217894901256aeab822f00/raw/b96d480d27d43f1bb11c3a0fe86f641070e6ce29/popular_names_ssa.csv).

#### My usual constraints

1. Build a version of the inspiration viz, simplifying if necessary to keep it relatively quick
1. Use only vanilla HTML, CSS, and JavaScript, as well as D3.js (I used v.5.0)
1. Where possible, use real-world raw data, and make it accessible if it's not already

Give it a try!
