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

### Using NumPy

The NumPy library simplifies this even further, providing a built-in method to calculate the mean automatically. Here's how to do it:

{%- highlight python -%}
    import numpy as np

    # Sample data
    data = [3, 4, 8, 9]

    # Calculate mean
    mean = np.mean(data)
    print(mean)

    #=> returns 6.0
{%- endhighlight -%}

### Using Pandas

Another helpful library commonly used in data analysis is Pandas. If you're working with data in a Pandas DataFrame, calculating the mean is just as simple as with NumPy. It also has a method you can call:

{%- highlight python -%}
    import pandas as pd

    # Sample data
    df = pd.DataFrame({'values': [1, 2, 3, 4]})

    # Calculate mean
    mean = df['values'].mean()
    print(mean)

    #=> returns 2.5
{%- endhighlight -%}

This simple measure gives a good sense of the "central" value of your data; however it can be sensitive to outliers in the dataset. Other descriptive statistics, such as the median and mode, can complement the mean and reveal a fuller picture of the story behind the data distribution.

# The Median

The median is another helpful statistic. It is the middle value of a dataset when the values are sorted. It helps us identify where the center-most values are in the data, and it is especially useful when your data has outliers or significant skew, as it's less sensitive to extreme values. In the case that your dataset has an even number of data points, the median can be calculated as the average of the two middle values. The median is quite a bit trickier to calculate in base Python than the median. Here's one (probably not optimized) way to do so:

{%- highlight python -%}
    # Sample data
    data = [10, 2, 30000, 400, 5000]
    n = len(data)

    # Sort the data:
    data.sort()

    # Check if even amount of data points
    if n % 2 == 0:
        middle_1 = int(n/2) - 1
        middle_2 = int(n/2)
        median = (data[middle_1] + data[middle_2]) / 2
        print(median)

    # If odd
    else:
        middle = int((n-1)/2)
        median = data[middle]
        print(median)

    #=> returns 400
{%- endhighlight -%}

### Using NumPy

Thankfully, our helpful Python libraries once again come to the rescue with a much simpler solution. Here's how to do so in NumPy:

{%- highlight python -%}
    import numpy as np

    # Sample data
    data = [18, 11, 13, 12, 14, 19]

    # Calculate median
    median = np.median(data)
    print(median)

    #=> returns 13.5
{%- endhighlight -%}

### Using Pandas

The Pandas library also has a very straightforward solution to find the median within a DataFrame:

{%- highlight python -%}
    import pandas as pd

    # Sample data
    df = pd.DataFrame({'values': [5, 10, 15, 20, 25]})

    # Calculate median
    median = df['values'].median()
    print(median)

    #=> returns 15
{%- endhighlight -%}

We can see from our first example that the data must be sorted first before selecting the middle value, otherwise it will be incorrect. Both the NumPy and Pandas functions do this automatically. Additionally we can see in the second example that since the amount of data is even, the average of the two middle values is returned! As explained earlier, the median is a better representation of central tendency in skewed datasets, as well as when outliers are present. While the mean is pulled towards extreme values, the median remains unaffected by them, which allows us to better understand the characteristics of our data!

# Standard Deviation

The standard deviation is another crucial statistic. It measures how spread out the values in a dataset are from the mean. Lower standard deviations indicate data that is tightly clustered around the mean, while higher means that the data is more spread out. It's an essential statistic for understanding the variability and consistency of your data. This one is even more tedious to calculate in base Python than the median:

{%- highlight python -%}
    # Sample data
    data = [1, 6, 13, 4, 18, 20, 22]
    n = len(data)

    # Calculate the mean
    data_sum = sum(data)
    mean = data_sum/n

    # Sum of squared differences between each data point and mean
    square_diff_sum = 0
    for i in range(n):
        square_diff_sum += (data[i] - mean) ** 2

    # Divide by n and take the square root
    variance = square_diff_sum/n
    std_dev = variance ** 0.5
    print(std_dev)

    #=> returns 7.764388
{%- endhighlight -%}

This code returns the population standard deviation. To calculate the standard deviation for a sample, simply replace the n in the variance variable calculation with (n-1).

### Using NumPy

The solution is significantly simpler and many magnitudes faster using one of our libraries like NumPy. Using NumPy, all we have to do is this:

{%- highlight python -%}
    import numpy as np

    std_dev = np.std(data)
    print(std_dev)

    #=> returns 7.764388
{%- endhighlight -%}

By default, np.std() calculates the population standard deviation. To calculate for a sample, you can set the ddof (Delta Degrees of Freedom) paramater to 1.

### Using Pandas

In Pandas, the standard deviation function std() calculates the sample standard deviation by default:

{%- highlight python -%}
    import pandas as pd

    # Sample data
    df = pd.DataFrame({'values': [5, 10, 15, 20, 25]})

    # Calculate sample standard deviation
    std_dev = df['values'].std()
    print(std_dev)

    #=> returns 7.90569
{%- endhighlight -%}

### Understanding the result

In the last example, we used the data [5, 10, 15, 20, 25]. The sample standard deviation calculated was approximately 7.9, which means each value deviates from the mean by about 7.9 units on average. 

### In conclusion

These 3 descriptive statistics are vitally important for understanding any set and distribution of data. They form a foundation upon which many more advanced statistical methods build. It's important to remember exactly what they represent in order to fully understand the meaning behind your data and distributions!

For a more full understanding of the ins and outs of the NumPy and Pandas libraries, check out the following links for their documentation:

(https://numpy.org/doc/)

(https://pandas.pydata.org/doc/)
