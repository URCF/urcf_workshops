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

# Repeating actions with loops

:::{admonition} Objectives
- Explain what a `for` loop does.
- Correctly write `for` loops to repeat simple calculations.
- Trace changes to a loop variable as the loop runs.
- Trace changes to other variables as they are updated by a `for` loop.
:::

:::{admonition} Questions
- How can I do the same operations on many different values?
:::

## Repeating Actions with Loops

So far, we've learned to access and manipulate individual elements of a list. But what if we want to perform the same operation on many different elements in the list (e.g. print them all out)? Or aggregate them somehow (e.g. calculate the total sum of all traffic)? This is where loops come in.

Let's say we have the following list of morning traffic counts, from 7am to 11am:

```python
morning_traffic = [500, 600, 550, 450, 350]
```

As we saw in the previous episode, we can access an element of a list using its index. For example, we can get the first number in the list `morning_traffic`, by using `morning_traffic[0]`. So one way to print each number is to use 5 `print` statements:

```{code-cell} python
morning_traffic = [500, 600, 550, 450, 350]
print(morning_traffic[0])
print(morning_traffic[1])
print(morning_traffic[2])
print(morning_traffic[3])
print(morning_traffic[4])
```

This is a bad approach for three reasons:

1. **Not scalable**. Imagine you need to print a list that has hundreds of elements. It might be easier to type them in manually, but it's tedious and error-prone.

2. **Difficult to maintain**. If we want to annotate each printed element somehow (e.g. add a unit), we would have to change multiple lines of code. While this might not be a problem for very small lists, it would definitely be a problem for longer ones.

3. **Fragile**. If we use it with a list that has more elements than what we initially envisioned, it will only display part of the list's elements. A shorter list, on the other hand, will cause an error because it will be trying to display elements of the list that do not exist.

Here's a better approach: a **for loop**

```{code-cell} python
for cars in morning_traffic:
    print(cars)
```

This is shorter, and more robust as well. For example, if we change the list by adding a new number at the end, we don't need to change the loop at all:

```{code-cell} python
morning_traffic.append(200)

for cars in morning_traffic:
    print(cars)
```

The general form of a loop is:

```python
for variable in collection:
    # do something using variable (e.g. print it)
```

We can call the **loop variable** anything we like, but there must be a colon at the end of the line starting the loop, and we must indent anything we want to run inside the loop. Unlike many other languages, there is no command to signify the end of the loop body (e.g. `end for`); everything indented after the `for` statement belongs to the loop.

~~~{admonition} What's in a name?
:class: tip

In the example above, the loop variable was given the name `cars` as a mnemonic; it is short for 'number of cars'. We can   choose any name we want for variables. We might just as easily have chosen the name `number` for the loop variable, as long as we use the same name when we invoke the variable inside the loop:

```{code-cell} python
morning_traffic = [500, 600, 550, 450, 350]
for number in morning_traffic:
    print(number)
```

It is a good idea to choose variable names that are meaningful, otherwise it would be more difficult to understand what the loop is doing.

~~~

~~~{admonition} Challenge: Printing with labels
:class: note

Given the list:

```python
morning_traffic = [500, 600, 550, 450, 350]
```

Write a loop that prints each number with "traffic: " before it. The output should be:

```output
traffic: 500
traffic: 600
traffic: 550
traffic: 450
traffic: 350
```

:::{dropdown} Solution
```python
for cars in morning_traffic:
    print('traffic: ', cars)
```
:::

~~~

## More complicated operations in loops

The operation inside the loop can be whatever we want. So far, we've only printed the values in the list, but loops can be used to perform more sophisticated calculations.

For example, what if we want to know how many hours of traffic we have recorded? Each entry in the list corresponds to one hour, so we to do this we can count the number of entries in the list.

```{code-cell} python
morning_traffic = [500, 600, 550, 450, 350]
entries = 0

for cars in morning_traffic:
    entries = entries + 1
print('Morning traffic hours:', entries)
```

It's worth tracing the execution of this little program step by step:

- Since there are 5 items in `morning_traffic`, the loop will be executed 5 times.

- The first time around, `entries` is zero (the value assigned to it at the start) and `cars` is `500`. The statement `entries = entries + 1` adds 1 to the old value of `entries`, producing 1, and updates `entries` to refer to that new value.
- The next time around, `cars` is `600` and `entries` is 1, so `entries` is updated to be 2.
- The next time, entries is 2 and cars is 550, so entries is updated to be 3.
- This process repeats for each entry in the list, until the loop has gone through all 5 items.
- After the loop has finished, `entries` is 5, so the program prints "Morning traffic hours: 5".

Note that finding the length of an object is such a common operation that Python actually has a built-in function to do it called `len`:

```{code-cell} python
print(len(morning_traffic))
```

Using `len` is faster and much more convenient than writing our own loop.

## Looping over other things

We can loop over any collection of several values, not just a list. For example, a string is a collection of it's individual characters. So we can loop over the letters in a string:

```{code-cell} python
time_of_day = 'morning'
for letter in time_of_day:
    print(letter)
```

~~~{admonition} Challenge: Summing a List
:class: note

Given the list:

```python
morning_traffic = [500, 600, 550, 450, 350]
```

Write a loop that adds up all the entries in `morning_traffic` to calculate the total traffic, and then prints the result. The output should be:

```output
Total traffic: 2450
```

:::{dropdown} Solution
```python
total = 0
for cars in morning_traffic:
    total = total + cars
print('Total traffic:', total)
```
:::

Just like there's a built-in `len` function to count the number of elements, Python also has a built-in `sum` function to quickly calculate the sum of a list:

```python
print('Total traffic:', sum(morning_traffic))
```

:::

~~~



~~~{admonition} Challenge: From 1 to N
:class: note

Python has a built-in function called `range` that generates a sequence of numbers. `range` can accept 1, 2, or 3 parameters.

- If one parameter is given, `range` generates a sequence of that length, starting at zero and incrementing by 1. For example, `range(3)` produces the numbers `0, 1, 2`.
- If two parameters are given, `range` starts at the first and ends just before the second, incrementing by one. For example, `range(2, 5)` produces `2, 3, 4`.

Using `range`, write a loop that prints numbers from 4 to 7. The output should be:

```python
4
5
6
7
```

:::{dropdown} Solution
```python
for number in range(4, 8):
    print(number)
```
:::

Now write a loop that prints the indices of the list `morning_traffic`. The output should be:

```output
0
1
2
3
4
```

*Hint*: Remember you can use `len(morning_traffic)` to get the length of the list.

:::{dropdown} Solution
```python
for i in range(len(morning_traffic)):
    print(i)
```
:::

~~~


~~~{admonition} Challenge: Dynamic Labels
:class: note

Given the list:
```python
morning_traffic = [500, 600, 550, 450, 350]
```

This list contains traffic counts for 7am, 8am, 9am, 10am, and 11am (in that order). Write a loop that prints each traffic count with a label showing which hour it's for. The output should be:

```
traffic at 7 am: 500
traffic at 8 am: 600
traffic at 9 am: 550
traffic at 10 am: 450
traffic at 11 am: 350
```

*Hint*: You'll need to use `range(len(morning_traffic))` to get the index, then use that index to both access the traffic count and calculate which hour it represents (remember: we started at 7am, so index 0 is 7am, index 1 is 8am, etc.).

:::{dropdown} Solution
```python
morning_traffic = [500, 600, 550, 450, 350]
for i in range(len(morning_traffic)):
    hour = 7 + i
    print('traffic at', hour, 'am:', morning_traffic[i])
```
:::

~~~

:::{admonition} Keypoints
- Use `for variable in sequence` to process the elements of a sequence one at a time.
- The body of a `for` loop must be indented.
- Use `len(thing)` to determine the length of something that contains other values.
:::
