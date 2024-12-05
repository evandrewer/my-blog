---
layout: post
title:  "Are Pokemon Obese? Part 2"
date: 04-12-2024
description: Using Streamlit to analyze Pokemon data across the different regions of the Pokemon world.
image: /assets/img/pokeball_cover.jpg
---

<p class="intro"><span class="dropcap">I</span>n my previous post, I explored some different attributes of Pokemon to discover trends about their types, heights, and weights. In this post, I'll showcase an app I made for simple exploratory data analysis with Streamlit, a Python library used for making custom web apps!</p>

<img src="{{site.url}}/{{site.baseurl}}/assets/img/bulbasaur.png" alt="" style="width:500px;"/>

## Key Insights

The main insight I have derived from my data is that, unsurprisingly, there seem to be few notable differences between types, heights, and weights across the 5 different regions. One difference that is unusual is the amount of Poison-type Pokemon in Kanto compared to the other regions. Across all 5, the Kanto region accounts for more than half of all Poison-types! 

<img src="{{site.url}}/{{site.baseurl}}/assets/img/pokegraph1.png" alt=""/>

Another strange finding is that the Pokemon in the Unova region tend to be shorter than the other regions by a noticeable margin.

<img src="{{site.url}}/{{site.baseurl}}/assets/img/pokegraph2.png" alt=""/>

## The App

You can check out my Streamlit app yourself right [here](https://evpokedata.streamlit.app/)! 

I made this app to simplify visualizing my dataset and allow for easier analysis. Users can select different regions and explore graphs displaying type, height, weight, and height vs weight. It contains graphs that will adjust based on selected regions in order to discover the relationships between each region!

For example, here's the height/weight correlation graph with all regions selected:

<img src="{{site.url}}/{{site.baseurl}}/assets/img/pokegraphallreg.png" alt=""/>

And here it is with just the Hoenn regions selected:

<img src="{{site.url}}/{{site.baseurl}}/assets/img/pokegraphhoenn.png" alt=""/>