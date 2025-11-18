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

# Python fundamentals

At its simplest, Python can be used as a calculator. Throughout this workshop, we're going to be working with traffic data from an automated sensor that measures how many cars passed in a given hour.

Let's say we're interested in looking at morning rush hour traffic, which we take to be the 7am and 8am measurements. On the day in question, there were 500 cars at 7am, and 600 cars at 8am. What's the total? We can use Python to do this addition.

```{code-cell} python
500 + 600
```

or any other arithmetic, for example, subtracting to get the difference:

```{code-cell} python
600 - 500
```

This fine, but we can do all this with a calculator app. To start making use of the power of a full-featured programming language like Python, we can assign our data to *variables*.

## Variables

We *assign* a value to a variable using the equals sign `=`. For example, we can track the number of cars at different times:

```{code-cell} python
cars_at_7am = 500
cars_at_8am = 600
```

From now on, whenever we use `cars_at_7am`, Python will substitute the value we assigned to it. You can think of it like this: **a variable is a name for a value**.

```{tip}
In Python, variable names:

- can include letters, digits, and underscores
- cannot start with a digit
- are case sensitive.

This means that, for example:

- `cars_7am` is a valid variable name, whereas `7am_cars` is not
- `cars` and `Cars` are different variables
```

## Types of data

Python knows various types of data. Three common ones are:

- integer numbers
- floating point (a.k.a. decimal) numbers, and
- text. In programming textual data is called "strings".

In the example above, variable `cars_at_7am` has a integer value of `500`. If we want to represent a decimal number, we can use a floating point:

```{code-cell} python
speed_mph = 54.3
```

To create a string, we add single or double quotes around some text. We might want a label for this group of data, for example:

```{code-cell} python
time_of_day = 'morning'
```

## Using Variables in Python

Once we have data stored with variable names, we can make use of it in calculations. Now we can do the same calculation of the total rush hour traffic, but using variables:

```{code-cell} python
cars_at_7am + cars_at_8am
```

This makes it more clear what we're doing.

We can also "add" strings together to create a new string. For example, we might want to make our label more descriptive:

```{code-cell} python
time_of_day + ' rush hour'
```

We can also store the results of calculations in variables:

```{code-cell} python
increase_in_cars = cars_at_8am - cars_at_7am
```

## Built-in Python functions

You might notice that when we assign a value to a variable, the value is not printed to the screen. To display the value of a variable, we can use the `print` function:

```{code-cell} python
print(increase_in_cars)
```

When we want to use a function, referred to as *calling* the function,
we type its name, followed by parentheses. The parentheses are important: if you
leave them off, the function doesn't actually run! Often you will include
values or variables inside the parentheses for the function to use. These are
called *arguments*, and including them is called *passing arguments* to a
function. In the case of `print`, we pass the `increase_in_cars` variable as an
argument to the `print` function, and `print` displays the value `increase_in_cars`.

We can display multiple things at once by passing multiple arguments:

```{code-cell} python
print("Cars at 7 AM:", cars_at_7am)
```

We can also call a function inside of another function call. For example, Python has a built-in function called `type` that tells you a value's data type:

```{code-cell} python
print(type(400))
print(type("morning"))
```

Moreover, we can do arithmetic with variables right inside the `print` function:

```{code-cell} python
print('Total rush hour traffic:', cars_at_7am + cars_at_8am)
```

~~~{admonition} Variables as Sticky Notes
:class: tip

A variable in Python is analogous to a sticky note with a name written on it: assigning a value to a variable is like putting that sticky note on a particular value.

<!-- TODO re-create these images with the traffic data rather than weight -->
<!-- ![Value of 500 with cars_at_7am label stuck on it](../fig/python_programming/01-intro/python-sticky-note-variables-01.svg) -->

Using this analogy, we can investigate how assigning a value to one variable does **not** change values of other, seemingly related, variables. For example, let's store the total rush hour traffic in its own variable:

```python
total_rush_hour = cars_at_7am + cars_at_8am
print('Cars at 7 AM:', cars_at_7am, 'Total rush hour:', total_rush_hour)
```

<!-- ![Value of 500 with cars_at_7am label stuck on it, and value of 1100 with total_rush_hour label stuck on it](../fig/python_programming/01-intro/python-sticky-note-variables-02.svg) -->

Similar to above, the expression `cars_at_7am + cars_at_8am` is evaluated to `1100`, and then this value is assigned to the variable `total_rush_hour` (i.e. the sticky note `total_rush_hour` is placed on `1100`). At this point, each variable is "stuck" to completely distinct and unrelated values.

Let's now change `cars_at_7am`:

```python
cars_at_7am = 600
print('Cars at 7 AM is now:', cars_at_7am, 'and total rush hour is still:', total_rush_hour)
```

<!-- ![Value of 600 with label cars_at_7am stuck on it, and value of 1100 with label total_rush_hour stuck on it](../fig/python_programming/01-intro/python-sticky-note-variables-03.svg) -->

Since `total_rush_hour` doesn't "remember" where its value comes from, it is not updated when we change `cars_at_7am`.

~~~

~~~{admonition} Challenge: Calculating Traffic Statistics
:class: note

You have traffic data for three hours: 450 cars at 9am, 550 cars at 10am, and 500 cars at 11am.

1. Create variables to store these three values.
2. Calculate and print the total traffic across all three hours.
3. Calculate and print the average traffic per hour (hint: divide the total by 3).

:::{dropdown} Solution

```python
traffic_9am = 450
traffic_10am = 550
traffic_11am = 500

total = traffic_9am + traffic_10am + traffic_11am
print('Total traffic:', total)

average = total / 3
print('Average traffic per hour:', average)
```

:::

~~~

~~~{admonition} Challenge: Using Print with Labels
:class: note

Create variables for the traffic counts at 7am (500) and 8am (600). Then use `print()` to display a message that shows both values with descriptive labels. The output should look like:

```
Traffic at 7am: 500
Traffic at 8am: 600
```

:::{dropdown} Solution

```python
traffic_7am = 500
traffic_8am = 600

print('Traffic at 7am:', traffic_7am)
print('Traffic at 8am:', traffic_8am)
```

:::

~~~

~~~{admonition} Challenge: Identifying Data Types
:class: note

What are the data types of the following variables? Use the `type()` function to check your answers.

```python
hour = 7
traffic_count = 500
location = 'Highway 101'
average_speed = 55.5
```

:::{dropdown} Solution

```python
hour = 7
traffic_count = 500
location = 'Highway 101'
average_speed = 55.5

print(type(hour))
print(type(traffic_count))
print(type(location))
print(type(average_speed))
```

`hour` and `traffic_count` are integers (`int`), `location` is a string (`str`), and `average_speed` is a floating-point number (`float`).

:::

~~~
