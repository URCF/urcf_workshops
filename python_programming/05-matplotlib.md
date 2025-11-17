# Visualizing Traffic Data

:::{admonition} Objectives
- Plot simple graphs from data.
- Plot multiple graphs in a single figure.
:::

:::{admonition} Questions
- How can I visualize tabular data in Python?
- How can I group several plots together?
:::

## Visualizing data

Now we can compute statistics, but it's often way more helpful and intuitive to visualize data with plots and graphs rather than just looking at numbers. Visualization deserves an entire workshop of its own, but we'll get started here with the basics. To do that, we're going to use a library called [`matplotlib`](https://matplotlib.org/). Like NumPy, `matplotlib` is not built-in to Python, but it's the most popular plotting library, so it's the *de facto* standard.

:::{admonition} Episode Prerequisites
If you are continuing in the same notebook from the previous episode, you already have a `traffic_data` variable and have imported `numpy`. If you are starting a new notebook at this point, you need the following two lines:

```python
import numpy
traffic_data = numpy.loadtxt('traffic_data.txt', delimiter=',')
```
:::

## Making Simple Plots with Matplotlib

Numbers are great, but a picture is worth a thousand words. Let's plot the traffic pattern for a single day. We can get the data for Tuesday by accessing the first row of our data (index 0).

First, we will import the `pyplot` module from `matplotlib` and use it to create and display a line graph:

```python
import matplotlib.pyplot
tuesday_data = traffic_data[0]
matplotlib.pyplot.plot(tuesday_data)
matplotlib.pyplot.show()
```

![A line graph showing the traffic pattern for Tuesday (day 0) over 24 hours. The plot shows two clear "humps" for the morning and evening commutes, with low traffic in the early morning hours and late night hours.](../fig/python_programming/03-matplotlib/traffic-single-day.svg)

The plot will show two clear "humps" for the morning and evening commutes. This is the first major insight where the data's shape has an obvious meaning—we can see the rush hour patterns!

Now let's take a look at the average traffic over time across all days:

```python
ave_traffic = numpy.mean(traffic_data, axis=0)
matplotlib.pyplot.plot(ave_traffic)
matplotlib.pyplot.show()
```

![A line graph showing the average traffic across all days over a 24-hour period. The plot shows the typical daily pattern with morning and evening rush hours.](../fig/python_programming/03-matplotlib/traffic-average.svg)

Here, we have put the average traffic per hour across all days in the variable `ave_traffic`, then asked `matplotlib.pyplot` to create and display a line graph of those values. The result shows the typical daily traffic pattern with clear morning and evening peaks.

But a good data scientist doesn't just consider the average of a dataset, so let's have a look at two other statistics:

```python
matplotlib.pyplot.plot(numpy.max(traffic_data, axis=0))
matplotlib.pyplot.show()
```

![A line graph showing the maximum traffic across all days over a 24-hour period.](../fig/python_programming/03-matplotlib/traffic-maximum.svg)

```python
matplotlib.pyplot.plot(numpy.min(traffic_data, axis=0))
matplotlib.pyplot.show()
```

![A line graph showing the minimum traffic across all days over a 24-hour period.](../fig/python_programming/03-matplotlib/traffic-minimum.svg)

These plots show us the range of traffic patterns—the maximum and minimum values for each hour across all days. This gives us insight into how much variation there is in the traffic patterns.

If we want, we can even look at all three graphs on the same plot:

```python
matplotlib.pyplot.plot(numpy.mean(traffic_data, axis=0))
matplotlib.pyplot.plot(numpy.max(traffic_data, axis=0))
matplotlib.pyplot.plot(numpy.min(traffic_data, axis=0))
matplotlib.pyplot.show()
```

![A line graph showing all three statistics (mean, max, min) across all days over a 24-hour period.](../fig/python_programming/03-matplotlib/traffic-all-stats.png)

This combined plot helps us see how the average relates to the extremes at each hour of the day.

```{admonition} Importing libraries with shortcuts
:class: tip

In this lesson we use the `import matplotlib.pyplot` **syntax** to import the `pyplot` module of `matplotlib`. However, shortcuts such as `import matplotlib.pyplot as plt` are frequently used. Importing `pyplot` this way means that after the initial import, rather than writing `matplotlib.pyplot.plot(...)`, you can now write `plt.plot(...)`. Another common convention is to use the shortcut `import numpy as np` when importing the NumPy library. We then can write `np.loadtxt(...)` instead of `numpy.loadtxt(...)`, for example.

Some people prefer these shortcuts as it is quicker to type and results in shorter lines of code - especially for libraries with long names! You will frequently see Python code online using a `pyplot` function with `plt`, or a NumPy function with `np`, and it's because they've used this shortcut. It makes no difference which approach you choose to take, but you must be consistent as if you use `import matplotlib.pyplot as plt` then `matplotlib.pyplot.plot(...)` will not work, and you must use `plt.plot(...)` instead. Because of this, when working with other people it is important you agree on how libraries are imported.

```


~~~{admonition} Challenge: Make Your Own Plot
:class: note

Create a plot showing the standard deviation (`numpy.std`) of the traffic data for each hour across all days.

:::{dropdown} Solution
```python
matplotlib.pyplot.plot(numpy.std(traffic_data, axis=0))
matplotlib.pyplot.show()
```

This will show you how much variation there is in traffic at each hour of the day.
:::

~~~

~~~{admonition} Challenge: Plotting Multiple Days
:class: note

Try plotting the traffic patterns for the first three days on the same plot. You can access each day using `traffic_data[0]`, `traffic_data[1]`, and `traffic_data[2]`.

:::{dropdown} Solution
```python
matplotlib.pyplot.plot(traffic_data[0])
matplotlib.pyplot.plot(traffic_data[1])
matplotlib.pyplot.plot(traffic_data[2])
matplotlib.pyplot.show()
```

This will show you how different days compare to each other.
:::

~~~

```{admonition} Keypoints
- Use the `pyplot` module from the `matplotlib` library for creating simple visualizations.
- Use `matplotlib.pyplot.plot()` to create line graphs.
- Use `matplotlib.pyplot.show()` to display plots.
```
