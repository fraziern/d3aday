---
layout: post
title:  "Generative Art I"
date:   2019-12-10 06:38:38 -0500
tags: generative-art nice-colors
thumbnail_path: /assets/img/generative_1_inspiration_1.jpg
thumbnail_alttext: Generative Art I
index: 07
---
For this exercise, I took inspiration from Philippe "Fil" Rivière's take on climate change in Minneapolis on [Observable](https://observablehq.com/@fil/summer-heat).

![Temperatures in Minneapolis-Saint Paul](/assets/img/summerheat_inspiration.jpg)

I liked how Fil's viz makes use of [small multiples](https://en.wikipedia.org/wiki/Small_multiple) to combine two types of graphs. The trend lines are of course a key feature of his data story, but they introduce more complexity into the code than I wanted for this quick exercise. 

For my version, I wanted to use data from my hometown instead of Minneapolis. I was able to find [historical monthly temperatures](https://sercc.com/cgi-bin/sercc/cliMONtavt.pl?nc7079) for Raleigh, and used Python to scrape and munge the data into something usable.

If you're interested, here's the scaper code. I love using Python for tasks like this.

{%highlight python %}
import requests
from bs4 import BeautifulSoup

page = requests.get("https://sercc.com/cgi-bin/sercc/cliMONtavt.pl?nc7079")
soup = BeautifulSoup(page.content, 'html.parser')

table = soup.find('table')
rows = table.find_all('tr')
data = []

for row in rows:
    cols = row.find_all('td')
    cols = [ele.text.strip() for ele in cols]
    if len(cols) > 14:
        cols = [cols[0]] + cols[1::2]
    data.append(cols)

data = data[:93] # truncate the summary statistics
{% endhighlight %}

And here's my solution:

![Temperatures in Raleigh, NC](/assets/img/summerheat_solution.jpg)

A key technique for this was to calculate the x coordinates for each dot by combining 2 scales, e.g.:

{%highlight JavaScript %}
let x = date =>
    xMonth(date.getMonth()) + xYear(date.getFullYear());
{% endhighlight %}

Complete code can be found on [codepen](https://codepen.io/fraziern/pen/rNaOdQO). Raleigh temperature data is [here](https://gist.githubusercontent.com/Fil/15e57d2584b618521d173d4c0088d13b/raw/2f7f1a236c074635435cc7ebf9253c20a5681690/data.csv).

### My usual constraints

1. Build a version of the inspiration viz, simplifying if necessary to keep it relatively quick
1. Use only vanilla HTML, CSS, and JavaScript, as well as D3.js (I used v.5.0)
1. Where possible, use real-world raw data, and make it accessible if it's not already

Give it a try!