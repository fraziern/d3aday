---
layout: post
title:  "Baby Names I"
date:   2019-12-10 06:38:38 -0500
tags: animation
thumbnail_path: /assets/img/babynames_solution.jpg
thumbnail_alttext: Baby Names I
index: 05
---

In this exercise, I wanted to practice some of the dynamic features of d3, like [transitions](https://github.com/d3/d3-transition) and the enter/update/exit lifecycle of [selection.join](https://observablehq.com/@d3/selection-join).

My initial inspiration came from the omnipresent [Bar Chart Race](https://app.flourish.studio/@flourish/bar-chart-race) visualizations, where a bar chart is animated to show rankings and volumes across time.

![Bar Chart Races](/assets/img/babynames_inspiration.jpg)

In the tradition of keeping it simple for these exercises, though, I got rid of the "bar chart" part of the bar chart race, and focused on the rankings part. I've always been fascinated by baby name popularity, so I generated a simple dataset from [SSA Baby Names Data](https://www.ssa.gov/oact/babynames/).

The idea is that the display dynamically shows the ranking of different names over a few years, with some names entering and some disappearing during that time. Here's what I ended up with:

![Baby Names](/assets/img/babynames_solution.gif)

Complete code and working example can be found on [codepen](https://codepen.io/fraziern/pen/vYEmyvx). The baby name data I used is [here](https://gist.githubusercontent.com/fraziern/6ca21ed36b217894901256aeab822f00/raw/b96d480d27d43f1bb11c3a0fe86f641070e6ce29/popular_names_ssa.csv).

### My usual constraints

1. Build a version of the inspiration viz, simplifying if necessary to keep it relatively quick
1. Use only vanilla HTML, CSS, and JavaScript, as well as D3.js (I used v.5.0)
1. Where possible, use real-world raw data, and make it accessible if it's not already

Give it a try!