# Storing Multiple Values in Lists

:::{admonition} Objectives
- Explain what a list is.
- Create and index lists of simple values.
- Change the values of individual elements
- Append values to an existing list
- Reorder and slice list elements
- Create and manipulate nested lists
:::

:::{admonition} Questions
- How can I store many values together?
:::

In the previous episode, we analyzed a single file of clinical trial
inflammation data. After finding some peculiar and potentially suspicious trends
in the trial data we ask Dr. Maverick if they have performed any other clinical
trials. Surprisingly, they say that they have and provide us with 11 more CSV
files for a further 11 clinical trials they have undertaken since the initial
trial.

Our goal now is to process all the inflammation data we have, which means that we still have
eleven more files to go!

The natural first step is to collect the names of all the files that we have to process. In Python,
a list is a way to store multiple values together. In this episode, we will learn how to store
multiple values in a list as well as how to work with lists.

## Python lists

Unlike NumPy arrays, lists are built into the language so we do not have to load a library
to use them.
We create a list by putting values inside square brackets and separating the values with commas:

```python
odds = [1, 3, 5, 7]
print('odds are:', odds)
```

```output
odds are: [1, 3, 5, 7]
```

We can access elements of a list using indices -- numbered positions of elements in the list.
These positions are numbered starting at 0, so the first element has an index of 0.

```python
print('first element:', odds[0])
print('last element:', odds[3])
print('"-1" element:', odds[-1])
```

```output
first element: 1
last element: 7
"-1" element: 7
```

Yes, we can use negative numbers as indices in Python. When we do so, the index `-1` gives us the
last element in the list, `-2` the second to last, and so on.
Because of this, `odds[3]` and `odds[-1]` point to the same element here.

## Lists with different data types

Unlike NumPy arrays, which can only hold numbers, lists can hold any type of data.

```pthon
fruits = ["apple", "pear", "orange"]
```

Lists can even contain elements of different types. Example:

```python
sample_ages = [10, 12.5, 'Unknown']
```

There are many ways to change the contents of lists:

```python
odds[0] = 15
print("odds after changing first value:", odds)
```

```output
odds after changing first value: [15, 3, 5, 7]
```

```python
odds.append(11)
print('odds after adding a value:', odds)
```

```output
odds after adding a value: [15, 3, 5, 7, 11]
```

```python
removed_element = odds.pop(0)
print('odds after removing the first element:', odds)
print('removed_element:', removed_element)
```

```output
odds after removing the first element: [3, 5, 7, 11]
removed_element: 15
```

```python
odds.reverse()
print('odds after reversing:', odds)
```

```output
odds after reversing: [11, 7, 5, 3]
```

While modifying in place, it is useful to remember that Python treats lists in a
slightly counter-intuitive way.

If we make a list, (attempt to) copy it and then modify this list, we can cause
all sorts of trouble. This also applies to modifying the list using the above
functions:

```python
odds = [3, 5, 7]
primes = odds
primes.append(2)
print('primes:', primes)
print('odds:', odds)
```

```output
primes: [3, 5, 7, 2]
odds: [3, 5, 7, 2]
```

This is because Python lets us use multiple names to refer to the *same list*.
The variable names are like two sticky notes stuck to the same drawer (the
list). In the code above, we add `2` to the drawer labeled `primes`, and then
check the contents of the drawer labeled `odds`. We find `2` there, because
they're the same drawer—it just has two labels on it.

## Slicing lists and strings

Subsets of lists can be accessed by specifying ranges of values in brackets,
similar to how we accessed ranges of positions in a NumPy array. This is
commonly referred to as "slicing" the list.

```python
chromosomes = ['X', 'Y', '2', '3', '4']
autosomes = chromosomes[2:5]
print('autosomes:', autosomes)

last = chromosomes[-1]
print('last:', last)
```

```output
autosomes: ['2', '3', '4']
last: 4
```

We can also slice strings in the same way:

```python
binomial_name = 'Drosophila melanogaster'
group = binomial_name[0:10]
print('group:', group)

species = binomial_name[11:23]
print('species:', species)
```

```output
group: Drosophila
species: melanogaster
```

~~~{admonition} Challenge: Slicing From the End
:class: note

Use slicing to access only the last four entries of a list.

```python
list_for_slicing = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

```output
[7, 8, 9, 10]
```

Would your solution work regardless of whether you knew beforehand
the length of the string or list
(e.g. if you wanted to apply the solution to a set of lists of different lengths)?
If not, try to change your approach to make it more robust.

*Hint*: Remember that indices can be negative as well as positive.

:::{dropdown} Solution
Use negative indices to count elements from the end of a container (such as list or string):

```python
list_for_slicing[-4:]
```
:::

~~~

~~~{admonition} Challenge: Overloading
:class: note

`+` usually means addition, but when used on strings or lists, it means "concatenate".
Given that, what do you think the multiplication operator `*` does on lists?
In particular, what will be the output of the following code?

```python
counts = [2, 4, 6, 8, 10]
repeats = counts * 2
print(repeats)
```

1. `[2, 4, 6, 8, 10, 2, 4, 6, 8, 10]`
2. `[4, 8, 12, 16, 20]`
3. `[[2, 4, 6, 8, 10], [2, 4, 6, 8, 10]]`
4. `[2, 4, 6, 8, 10, 4, 8, 12, 16, 20]`

The technical term for this is *operator overloading*:
a single operator, like `+` or `*`,
can do different things depending on what it's applied to.

:::{dropdown} Solution
The multiplication operator `*` used on a list replicates elements of the list and concatenates
them together:

```output
[2, 4, 6, 8, 10, 2, 4, 6, 8, 10]
```

It's equivalent to:

```python
counts + counts
```
:::

~~~

:::{admonition} Keypoints
- `[value1, value2, value3, ...]` creates a list.
- Lists can contain any Python object, including lists (i.e., list of lists).
- Lists are indexed and sliced with square brackets (e.g., `list[0]` and `list[2:9]`), in the same way as strings and arrays.
- Lists are mutable (i.e., their values can be changed in place).
- Strings are immutable (i.e., the characters in them cannot be changed).
:::


