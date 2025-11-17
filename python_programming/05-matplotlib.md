---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

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

### Prerequisites

If you are continuing in the same notebook from the previous lesson, you already have a `traffic_data` variable and have imported `numpy`. If you are starting a new notebook at this point, you need to load the data and import the library:

```{code-cell} python
import numpy
traffic_data = numpy.loadtxt('traffic_data.txt', delimiter=',')
```

## Making Simple Plots with Matplotlib


Numbers are great, but a picture is worth a thousand words. Lets learn how to make simple plots in Python. For this, we use another library called `matplotlib`, specifically a *module* called `pyplot`. A *module* is a sub-part of a library—a collection of functions and variables that are related to a specific topic. `matplotlib` has other modules for more complex plots, but `pyplot` is the simplest and most commonly used.

We can use the `plot` function to create a line graph of a list of numbers:


```{code-cell} python
import matplotlib.pyplot

matplotlib.pyplot.plot([1, 4, 9, 16, 25])
```

The X-axis is the index of the list, and the Y-axis is the value at that index.

It's annoying to have to type out the `matplotlib.pyplot` module name every time, so we can import it with a shortcut to make that easier:

```{code-cell} python
import matplotlib.pyplot as plt

plt.plot([1, 4, 9, 16, 25])
```

The `as` keyword lets us give the imported module a shorter name. In this case, we've called it `plt`, and everytime we type `plt` from now on, Python will replace it with `matplotlib.pyplot`.

You also might notice that Python shows us a textual representation of the plot (`[<matplotlib.lines.Line2D at ...>]`) in addition to displaying it. We can suppress this by adding a semicolon to the end of the line:

```{code-cell} python
plt.plot([1, 4, 9, 16, 25]);
```

We'll do this in the rest of our plotting commands to avoid cluttering our output.

## Plotting traffic data

Now let's get to plotting our actual data. We can get just the first row of data (the first day) using indexing, and pass   this to the plot function:

```{code-cell} python
first_day_data = traffic_data[0]
plt.plot(first_day_data);
```

We could similarly look at the second day:

```{code-cell} python
second_day_data = traffic_data[1]
plt.plot(second_day_data);
```

Sketch of rest:

- Add title and axis labels
- Plot multiple days on a single plot
- Plot multiple days on separate plots
- Then, in cond section, distinguish weekdays and weekends
    - Use this to plot weekdays and weekends in different colors