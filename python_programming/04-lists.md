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

## The Story of a Single Day

In the previous episode, we worked with individual data points—single moments in time from our traffic data. Now we're going to scale up from individual data points to a complete series: one full day of traffic data.

We have the data for all 24 hours of a Tuesday. Instead of creating 24 variables like `hour_0`, `hour_1`, `hour_2`, and so on, we can store them all in order in a single list.

## Python lists

Unlike NumPy arrays, lists are built into the language so we do not have to load a library to use them. We create a list by putting values inside square brackets and separating the values with commas:

```python
tuesday_traffic = [598, 369, 312, 367, 835, 2726, 5689, 6990, 5985, 5309, 4603, 4884, 5104, 5178, 5501, 5713, 6292, 6057, 4907, 3503, 3037, 2822, 1992, 1166]
print('Tuesday traffic:', tuesday_traffic)
```

```output
Tuesday traffic: [598, 369, 312, 367, 835, 2726, 5689, 6990, 5985, 5309, 4603, 4884, 5104, 5178, 5501, 5713, 6292, 6057, 4907, 3503, 3037, 2822, 1992, 1166]
```

We can access elements of a list using indices -- numbered positions of elements in the list. These positions are numbered starting at 0, so the first element has an index of 0.

## Accessing List Items with Indexing

Now we can easily pull out specific information. Remember that Python starts counting from 0. What was the traffic at 2 PM, which is the 15th hour of the day (index 14)? What about at the start of the evening commute at 4 PM (index 16)?

```python
print('Traffic at 2 PM (index 14):', tuesday_traffic[14])
print('Traffic at 4 PM (index 16):', tuesday_traffic[16])
```

```output
Traffic at 2 PM (index 14): 5501
Traffic at 4 PM (index 16): 6292
```

Yes, we can use negative numbers as indices in Python. When we do so, the index `-1` gives us the last element in the list, `-2` the second to last, and so on:

```python
print('first element:', tuesday_traffic[0])
print('last element:', tuesday_traffic[23])
print('"-1" element:', tuesday_traffic[-1])
```

```output
first element: 598
last element: 1166
"-1" element: 1166
```

Because of this, `tuesday_traffic[23]` and `tuesday_traffic[-1]` point to the same element here.

## Lists with different data types

Unlike NumPy arrays, which can only hold numbers, lists can hold any type of data.

```python
traffic_labels = ["midnight", "1am", "2am", "3am"]
```

Lists can even contain elements of different types:

```python
mixed_data = [598, 'vehicles', 7.5]
```

There are many ways to change the contents of lists:

```python
tuesday_traffic[0] = 600
print("tuesday_traffic after changing first value:", tuesday_traffic[0])
```

```output
tuesday_traffic after changing first value: 600
```

```python
tuesday_traffic.append(1200)
print('tuesday_traffic after adding a value:', tuesday_traffic[-1])
```

```output
tuesday_traffic after adding a value: 1200
```

```python
removed_element = tuesday_traffic.pop(0)
print('tuesday_traffic after removing the first element:', tuesday_traffic[0])
print('removed_element:', removed_element)
```

```output
tuesday_traffic after removing the first element: 369
removed_element: 600
```

```python
tuesday_traffic.reverse()
print('tuesday_traffic after reversing:', tuesday_traffic)
```

```output
tuesday_traffic after reversing: [1166, 1992, 2822, 3037, 3503, 4907, 6057, 6292, 5713, 5501, 5178, 5104, 4884, 4603, 5309, 5985, 6990, 5689, 2726, 835, 367, 312, 369, 598]
```

While modifying in place, it is useful to remember that Python treats lists in a slightly counter-intuitive way.

If we make a list, (attempt to) copy it and then modify this list, we can cause all sorts of trouble. This also applies to modifying the list using the above functions:

```python
tuesday_traffic = [598, 369, 312, 367, 835, 2726]
wednesday_traffic = tuesday_traffic
wednesday_traffic.append(2800)
print('wednesday_traffic:', wednesday_traffic)
print('tuesday_traffic:', tuesday_traffic)
```

```output
wednesday_traffic: [598, 369, 312, 367, 835, 2726, 2800]
tuesday_traffic: [598, 369, 312, 367, 835, 2726, 2800]
```

This is because Python lets us use multiple names to refer to the *same list*. The variable names are like two sticky notes stuck to the same drawer (the list). In the code above, we add `2800` to the drawer labeled `wednesday_traffic`, and then check the contents of the drawer labeled `tuesday_traffic`. We find `2800` there, because they're the same drawer—it just has two labels on it.

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
