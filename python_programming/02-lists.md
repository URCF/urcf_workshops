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

# Storing Multiple Values in Lists

:::{admonition} Objectives
- Explain what a list is.
- Create and index lists of simple values.
- Change the values of individual elements
- Append values to an existing list
- Reorder list elements
:::

:::{admonition} Questions
- How can I store many values together?
:::

So far, we've worked with individual data points, with a variable holding a single number—the amount of traffic during a given hour. Now we're going to scale up from individual data points to a series of related values: an entire morning of traffic data.

Let's say we want to store and analyze the traffic data for an entire morning. We could create a separate variable for each hour:

```python
hour_7am = 500
hour_8am = 600
hour_9am = 550
hour_10am = 450
hour_11am = 350
```

But this would quickly get tedious (imagine trying to work with hourly data for a full week, or a year). Instead, we can store all of the data together in a list.

## Python lists

We create a list by putting values inside square brackets and separating the values with commas:

```{code-cell} python
morning_traffic = [500, 600, 550, 450, 350]
print(morning_traffic)
```

This list contains traffic counts for each hour of the morning, from 7am to 11am.

We can access elements of a list using indices—numbered positions of elements in the list.

These positions are numbered starting at 0, so the first element has an index of 0.

## Accessing List Items with Indexing

Now we can easily pull out specific information. What was the traffic at 7 AM (the first hour in our list, which is index 0)? What about at 8 AM (the second hour, which is index 1)?

```{code-cell} python
print('Traffic at 7 AM (index 0):', morning_traffic[0])
print('Traffic at 8 AM (index 1):', morning_traffic[1])
```

It might be surprising that we start counting from 0, rather than 1. Many programming languages start counting indicies from 0 because it represents an offset from the first value in the list (the second value is offset by one index from the first value). This is closer to the way that computers represent data internally. As a result, if we have a list with N elements in Python, its indices go from 0 to N-1. It takes a bit of getting used to, but one way to remember the rule is that the index is how many steps we have to take from the start to get the item we want.

~~~{admonition} Challenge: Negative Indexing
:class: note

Python also allows us to use negative numbers as indices. When we do so, we start counting from the end of the list rather than the beginning.

Given the list:
```python
morning_traffic = [500, 600, 550, 450, 350]
```

How would you get the last element of this list using negative indexing? How would you get the second to last element?

:::{dropdown} Solution

To get the last element, use index `-1`:
```{code-cell} python
print(morning_traffic[-1])
```

To get the second to last element, use index `-2`:
```{code-cell} python
print(morning_traffic[-2])
```

Negative indexing is particularly useful when you don't know the length of a list, or when you want to access elements from the end without having to calculate their position from the start.

:::

~~~

## Lists with different data types

Lists can hold any type of data. For example, we could use strings to store the time of day for each data point:

```python
time_labels = ["7am", "8am", "9am", "10am", "11am"]
```

Lists can even contain elements of different types:

```python
mixed_data = [500, 'vehicles', 7.5]
```

There are many ways to change the contents of lists.

In addition to reading from a list using an index, we can also change the value of an element in the list using an index combined with the `=` assignment operator:

```{code-cell} python
morning_traffic[0] = 700
print("morning_traffic after changing first value:", morning_traffic)
```

We can also add a new element to the end of a list using the `append` method:

```{code-cell} python
morning_traffic.append(200)
print('morning_traffic after adding a value:', morning_traffic)
```

Notice the *dot notation* (`.`) between the list name and `append`. We are calling a function called `append`, but it's a special function that belongs to the `morning_traffic` list. This type of function that's part of an object is called a *method*. Methods are accessed using dot notation. When you see `morning_traffic.append`, you can read it as "the `append` method that belongs to `morning_traffic`".

As an example, think of it like a family name: John Smith is the John that belongs to the Smith family. We could use dot notation to write his name as `smith.john`. `smith` is a container (family) that holds people, one of whom is `john`, just as `morning_traffic` is a container (list) that has methods, one of which is `append`.

List have lots of other methods that can be used to modify them. For example, the `reverse` method reverses the order of elements in the list:

```{code-cell} python
morning_traffic.reverse()
print('morning_traffic after reversing:', morning_traffic)
```

While modifying in place, it is useful to remember that Python treats lists in a slightly counter-intuitive way.

If we make a list, (attempt to) copy it and then modify this list, we can cause all sorts of trouble. This also applies to modifying the list using the above functions:

```{code-cell} python
morning_traffic = [500, 600, 550, 450, 350]
morning_traffic_copy = morning_traffic
morning_traffic_copy.append(200)
print('morning_traffic_copy:', morning_traffic_copy)
print('morning_traffic:', morning_traffic)
```

This is because Python lets us use multiple names to refer to the *same list*. The variable names are like two sticky notes stuck to the same drawer (the list). In the code above, we add `200` to the drawer labeled `morning_traffic_copy`, and then check the contents of the drawer labeled `morning_traffic`. We find `200` there, because they're the same drawer—it just has two labels on it.

~~~{admonition} Challenge: Overloading
:class: note

`+` usually means addition, but when used on strings or lists, it means "concatenate". Given that, what do you think the multiplication operator `*` does on lists? In particular, what will be the output of the following code?

```python
hourly_counts = [100, 200, 300]
repeats = hourly_counts * 2
print(repeats)
```

1. `[100, 200, 300, 100, 200, 300]`
2. `[200, 400, 600]`
3. `[[100, 200, 300], [100, 200, 300]]`
4. `[100, 200, 300, 200, 400, 600]`

The technical term for this is *operator overloading*: a single operator, like `+` or `*`, can do different things depending on what it's applied to.

:::{dropdown} Solution
The multiplication operator `*` used on a list replicates elements of the list and concatenates them together:

```output
[100, 200, 300, 100, 200, 300]
```

It's equivalent to:

```python
hourly_counts + hourly_counts
```
:::

~~~

:::{admonition} Keypoints
- `[value1, value2, value3, ...]` creates a list.
- Lists can contain any Python object, including lists (i.e., list of lists).
- Lists are indexed with square brackets (e.g., `list[0]`).
- Lists are mutable (i.e., their values can be changed in place).
- Strings are immutable (i.e., the characters in them cannot be changed).
:::
