# Repeating Actions with Loops

:::{admonition} Objectives
- Explain what a `for` loop does.
- Correctly write `for` loops to repeat simple calculations.
- Trace changes to a loop variable as the loop runs.
- Trace changes to other variables as they are updated by a `for` loop.
:::

:::{admonition} Questions
- How can I do the same operations on many different values?
:::

Remember: our goal is to do the same visualization we did on the first
inflammation dataset on all 12 datasets that Dr. Maverick has given us.

We'll use lists to collect all these datasets together, but we also need a way
to do the same analysis on each dataset. To do that, we'll have to teach the
computer how to repeat things.

An example task that we might want to repeat is accessing numbers in a list,
which we will do by printing each number on a line of its own.


```python
odds = [1, 3, 5, 7]
```

As we saw in the previous episode, we can access an element of a list using its
index. For example, we can get the first number in the list `odds`, by using
`odds[0]`. So one way to print each number is to use four `print` statements:

```python
print(odds[0])
print(odds[1])
print(odds[2])
print(odds[3])
```

```output
1
3
5
7
```

This is a bad approach for three reasons:

1. **Not scalable**. Imagine you need to print a list that has hundreds
  of elements.  It might be easier to type them in manually.

2. **Difficult to maintain**. If we want to decorate each printed element with an
  asterisk or any other character, we would have to change four lines of code. While
  this might not be a problem for small lists, it would definitely be a problem for
  longer ones.

3. **Fragile**. If we use it with a list that has more elements than what we initially
  envisioned, it will only display part of the list's elements. A shorter list, on
  the other hand, will cause an error because it will be trying to display elements of the
  list that do not exist.

```python
odds = [1, 3, 5]
print(odds[0])
print(odds[1])
print(odds[2])
print(odds[3])
```

```output
1
3
5
```

```error
---------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
<ipython-input-3-7974b6cdaf14> in <module>()
      3 print(odds[1])
      4 print(odds[2])
----> 5 print(odds[3])

IndexError: list index out of range
```

Here's a better approach: a **for loop**

```python
odds = [1, 3, 5, 7]
for num in odds:
    print(num)
```

```output
1
3
5
7
```

This is shorter --- certainly shorter than something that prints every number in a
hundred-number list --- and more robust as well:

```python
odds = [1, 3, 5, 7, 9, 11]
for num in odds:
    print(num)
```

```output
1
3
5
7
9
11
```

The improved version uses a **for loop**
to repeat an operation --- in this case, printing --- once for each thing in a sequence.
The general form of a loop is:

```python
for variable in collection:
    # do things using variable, such as print
```

Using the odds example above, the loop might look like this:

![Loop variable 'num' being assigned the value of each element in the list odds in turn andthen being printed](../fig/python_programming/05-loop/05-loops_image_num.png)

where each number (`num`) in the variable `odds` is looped through and printed one number after
another. The other numbers in the diagram denote which loop cycle the number was printed in (1
being the first loop cycle, and 6 being the final loop cycle).

We can call the **loop variable** anything we like, but
there must be a colon at the end of the line starting the loop, and we must indent anything we
want to run inside the loop. Unlike many other languages, there is no command to signify the end
of the loop body (e.g. `end for`); everything indented after the `for` statement belongs to the loop.

~~~{admonition} What's in a name?
:class: tip

In the example above, the loop variable was given the name `num` as a mnemonic;
it is short for 'number'.
We can choose any name we want for variables.  We might just as easily have chosen the name
`banana` for the loop variable, as long as we use the same name when we invoke the variable inside
the loop:

```python
odds = [1, 3, 5, 7, 9, 11]
for banana in odds:
    print(banana)
```

```output
1
3
5
7
9
11
```

It is a good idea to choose variable names that are meaningful, otherwise it would be more
difficult to understand what the loop is doing.

~~~


Here's another loop that repeatedly updates a variable:

```python
length = 0
names = ['Curie', 'Darwin', 'Turing']
for name in names:
    length = length + 1
print('There are', length, 'names in the list.')
```

```output
There are 3 names in the list.
```

It's worth tracing the execution of this little program step by step:

- Since there are three names in `names`, the loop will be executed three times.

- The first time around, `length` is zero (the value assigned to it at the start)
  and `name` is `Curie`. The statement adds 1 to the old value of `length`,
  producing 1, and updates `length` to refer to that new value.
- The next time around, `name` is `Darwin` and `length` is 1, so `length` is
  updated to be 2.
- After one more update, `length` is 3. Since there is nothing left in `names`
  for Python to process, the loop finishes and the `print` function on line 5
  tells us our final answer.

Note that finding the length of an object is such a common operation that Python
actually has a built-in function to do it called `len`:

```python
print(len([0, 1, 2, 3]))
```

```output
4
```

`len` is much faster than any function we could write ourselves, and much easier
to read than a two-line loop, so we should always use it when we can.

## Looping over other things

We can loop over any collection of values, not just a list. For example, we can loop over the letters in a string:

```python
word = 'cow'
for letter in word:
    print(letter)
```

```output
c
o
w
```

~~~{admonition} Challenge: Summing a list
:class: note

Let's say we have the following list:

```python
numbers = [124, 402, 36]
```

Write a loop that calculates the sum of elements in this list
by adding each element to a running total and printing the final value.
So for this list of numbers, `[124, 402, 36]`, your code should print `562`.

:::{dropdown} Solution
```python
numbers = [124, 402, 36]
total = 0
for num in numbers:
    total = total + num
print(total)
```
:::
~~~

~~~{admonition} Challenge: Understanding loops
:class: note

Given the following loop:

```python
word = 'oxygen'
for letter in word:
    print(letter)
```

How many times is the body of the loop executed?

- 3 times
- 4 times
- 5 times
- 6 times

:::{dropdown} Solution
The body of the loop is executed six times. This is because it's executed once per each character in the word "oxygen", which is six characters long.
:::
~~~

~~~{admonition} Challenge: From 1 to N
:class: note

Python has a built-in function called `range` that generates a sequence of numbers. `range` can
accept 1, 2, or 3 parameters.

- If one parameter is given, `range` generates a sequence of that length,
  starting at zero and incrementing by 1.
  For example, `range(3)` produces the numbers `0, 1, 2`.
- If two parameters are given, `range` starts at
  the first and ends just before the second, incrementing by one.
  For example, `range(2, 5)` produces `2, 3, 4`.
- If `range` is given 3 parameters,
  it starts at the first one, ends just before the second one, and increments by the third one.
  For example, `range(3, 10, 2)` produces `3, 5, 7, 9`.

Using `range`,
write a loop that prints the first 3 natural numbers:

```python
1
2
3
```

:::{dropdown} Solution
```python
for number in range(1, 4):
    print(number)
```
:::
~~~

~~~{admonition} Challenge: Computing Powers With Loops
:class: note
Exponentiation is built into Python:

```python
print(5 ** 3)
```

```output
125
```

Write a loop that calculates the same result as `5 ** 3` using
multiplication (and without exponentiation).

:::{dropdown} Solution
```python
result = 1
for number in range(0, 3):
    result = result * 5
print(result)
```
:::
~~~

:::{admonition} Keypoints
- Use `for variable in sequence` to process the elements of a sequence one at a time.
- The body of a `for` loop must be indented.
- Use `len(thing)` to determine the length of something that contains other values.
:::


