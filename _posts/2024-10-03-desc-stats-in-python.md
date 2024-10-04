---
layout: post
title:  "Calculating Basic Descriptive Statistics in Python"
date: 03-10-2024
description: This is a tutorial for how to calculate and use basic statistics to summarize your data.
image: /assets/img/blog-image.jpg
---

<p class="intro"><span class="dropcap">D</span>escriptive statistics are a crucial tool for summarizing and understanding data. Whether you're analyzing sales, results from surveys, or data from personal projects, understanding basic metrics like the mean, median, and standard deviation can provide much needed insight.

In this post, I’ll introduce you to the necessary functions and packages needed to calculate these statistics using Python. With the assistance of often-used libraries like NumPy and Pandas, you’ll know how to summarize and interpret your data with just a few lines of code. Once you’ve finished reading, you’ll be ready to make sense of your datasets and perform fundamental data analysis tasks with confidence! </p>

# The Mean

The mean is perhaps the most commonly used descriptive statistic for data. It gives us an idea of the central value of a set of data by adding each value in the dataset together and dividing by the total number of data points. We can caclulate this in base Python using code like the following:

{%- highlight python -%}
    # Sample data
    data = [4, 8, 2, 3]
	
    # Calculate the sum and count
    data_sum = sum(data)
    n = len(data)

    # Calculate the mean
    mean = data_sum/n
    print(mean)

    #=> returns 4.25
{%- endhighlight -%}

## Using NumPy

The NumPy library simplifeis this even further, providing a built-in method to calculate the mean automatically. Here's how to do it:

{%- highlight python -%}
    import numpy as np

    # Sample data
    data = [3, 4, 8, 9]

    # Calculate mean
    mean = np.mean(data)
    print(mean)

    #=> returns 6.0
{%- endhighlight -%}


