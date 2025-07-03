# Making Choices

:::{admonition} Objectives

- Write conditional statements including `if`, `elif`, and `else` branches.
- Correctly evaluate expressions containing `and` and `or`.

:::

:::{admonition} Questions

- How can my programs do different things based on data values?

:::

In our last lesson, we discovered something suspicious was going on
in our inflammation data by drawing some plots.
How can we use Python to automatically recognize the different features we saw,
and take a different action for each? In this lesson, we'll learn how to write code that
runs only when certain conditions are true.

## Conditionals

We can ask Python to take different actions, depending on a condition, with an `if` statement:

```python
num = 37
if num > 100:
    print('greater')
else:
    print('not greater')
print('done')
```

```output
not greater
done
```

The second line of this code uses the keyword `if` to tell Python that we want to make a choice.
If the test that follows the `if` statement is true,
the body of the `if`
(i.e., the set of lines indented underneath it) is executed, and "greater" is printed.
If the test is false,
the body of the `else` is executed instead, and "not greater" is printed.
Only one or the other is ever executed before continuing on with program execution to print "done":

![](../fig/python_programming/python-flowchart-conditional.png)

Conditional statements don't have to include an `else`. If there isn't one,
Python simply does nothing if the test is false:

```python
num = 53
print('before conditional...')
if num > 100:
    print(num, 'is greater than 100')
print('...after conditional')
```

```output
before conditional...
...after conditional
```

We can also chain several tests together using `elif`,
which is short for "else if".
The following Python code uses `elif` to print the sign of a number.

```python
num = -3

if num > 0:
    print(num, 'is positive')
elif num == 0:
    print(num, 'is zero')
else:
    print(num, 'is negative')
```

```output
-3 is negative
```

Note that to test for equality we use a double equals sign `==`
rather than a single equals sign `=` which is used to assign values.

:::{admonition} Comparing in Python

Along with the `>` and `==` operators we have already used for comparing values in our
conditionals, there are a few more options to know about:

- `>`: greater than
- `<`: less than
- `==`: equal to
- `!=`: does not equal
- `>=`: greater than or equal to
- `<=`: less than or equal to

:::

We can also combine tests using `and` and `or`.
`and` is only true if both parts are true:

```python
if 7 > 5 and 2 >= 5:
    print('both parts are true')
else:
    print('at least one part is false')
```

```output
at least one part is false
```

while `or` is true if at least one part is true:

```python
if 7 > 7 or 2 >= 5:
    print('at least one test is true')
```

```output
at least one test is true
```

## `True` and `False`

`True` and `False` are special words in Python called `booleans`, which
represent truth values. A statement such as `1 < 0` returns the value `False`,
while `-1 < 0` returns the value `True`. For example:


```python
print(1 < 0)
```

```output
False
```


## Checking our Data

Now that we've seen how conditionals work,
we can use them to check for the suspicious features we saw in our inflammation data.
We are about to use functions provided by the `numpy` module again.
Therefore, if you're working in a new Python session, make sure to load the
module with:

```python
import numpy
```

From the first couple of plots, we saw that maximum daily inflammation has a
perfectly linear rise and fall. Wouldn't it be a good idea to detect such
behavior and report it as suspicious? Let's do that! However, instead of
checking every single day of the study, let's merely check if maximum
inflammation in the beginning (day 0) and in the middle (day 20) of the study
are equal to the corresponding day numbers.

```python
max_inflammation_0 = numpy.max(data, axis=0)[0]
max_inflammation_20 = numpy.max(data, axis=0)[20]

if max_inflammation_0 == 0 and max_inflammation_20 == 20:
    print('Suspicious looking maxima!')
```

We also saw a different problem in the third dataset; the minima per day were
all zero (looks like a healthy person snuck into our study). We can also check
for this with an `elif` condition:

```python
elif numpy.sum(numpy.min(data, axis=0)) == 0:
    print('Minima add up to zero!')
```

And if neither of these conditions are true, we can use `else` to give the all-clear:

```python
else:
    print('Seems OK!')
```

Let's test that out:

```python
data = numpy.loadtxt('inflammation-01.csv', delimiter=',')

max_inflammation_0 = numpy.max(data, axis=0)[0]
max_inflammation_20 = numpy.max(data, axis=0)[20]

if max_inflammation_0 == 0 and max_inflammation_20 == 20:
    print('Suspicious looking maxima!')
elif numpy.sum(numpy.min(data, axis=0)) == 0:
    print('Minima add up to zero!')
else:
    print('Seems OK!')
```

```output
Suspicious looking maxima!
```

```python
data = numpy.loadtxt('inflammation-03.csv', delimiter=',')

max_inflammation_0 = numpy.max(data, axis=0)[0]
max_inflammation_20 = numpy.max(data, axis=0)[20]

if max_inflammation_0 == 0 and max_inflammation_20 == 20:
    print('Suspicious looking maxima!')
elif numpy.sum(numpy.min(data, axis=0)) == 0:
    print('Minima add up to zero!')
else:
    print('Seems OK!')
```

```output
Minima add up to zero!
```

In this way, we have asked Python to do something different depending on the
condition of our data. Here we printed messages in all cases, but we could also
imagine not using the `else` catch-all so that messages are only printed when
something is wrong, freeing us from having to manually examine every plot for
features we've seen before.

~~~{admonition} Challenge: How Many Paths?

Consider this code:

```python
if 4 > 5:
    print('A')
elif 4 == 5:
    print('B')
elif 4 < 5:
    print('C')
```

Which of the following would be printed if you were to run this code?
Why did you pick this answer?

1. A
2. B
3. C
4. B and C

:::{dropdown} Solution

C gets printed because the first two conditions, `4 > 5` and `4 == 5`, are not true,
but `4 < 5` is true.
In this case only one of these conditions can be true for at a time, but in other
scenarios multiple `elif` conditions could be met. In these scenarios only the action
associated with the first true `elif` condition will occur, starting from the top of the
conditional section.
![](../fig/python_programming/python-else-if.png)
This contrasts with the case of multiple `if` statements, where every action can occur
as long as their condition is met.
![](../fig/python_programming/python-multi-if.png)
:::

~~~

~~~{admonition} Challenge: What Is Truth?

`True` and `False` booleans are not the only values in Python that are true and
false. In fact, *any* value can be used in an `if` or `elif`. After reading and
running the code below, explain what the rule is for which values are considered
true and which are considered false.

```python
if '':
    print('empty string is true')
if 'word':
    print('word is true')
if []:
    print('empty list is true')
if [1, 2, 3]:
    print('non-empty list is true')
if 0:
    print('zero is true')
if 1:
    print('one is true')
```

:::{dropdown} Solution
The rule is that any non-zero number is true, any empty collection (like an empty string or list) is false, and any non-empty collection is true.
:::

~~~

~~~{admonition} Challenge: That's Not Not What I Meant

Sometimes it is useful to check whether some condition is not true.
The Boolean operator `not` can do this explicitly.
After reading and running the code below,
write some `if` statements that use `not` to test the rule
that you formulated in the previous challenge.

:::{dropdown} Solution
```python
if not '':
    print('empty string is not true')
if not 'word':
    print('word is not true')
if not not True:
    print('not not True is true')
```
:::

~~~

~~~{admonition} Challenge: Counting Vowels

1. Write a loop that counts the number of vowels in a character string.
2. Test it on a few individual words and full sentences.
3. Once you are done, compare your solution to your neighbor's.
  Did you make the same decisions about how to handle the letter 'y'
  (which some people think is a vowel, and some do not)?

*Hint*: You can check for membership in a collection using the keyword `in`. For example:

```python
if 'a' in 'aardvark':
    print('a is in aardvark')
```

```output
a is in aardvark
```

:::{dropdown} Solution
```python
vowels = 'aeiouAEIOU'
sentence = 'Mary had a little lamb.'
count = 0
for char in sentence:
    if char in vowels:
        count += 1

print('The number of vowels in this string is ' + str(count))
```
:::

~~~


:::{admonition} Keypoints

- Use `if condition` to start a conditional statement, `elif condition` to provide additional tests, and `else` to provide a default.
- The bodies of the branches of conditional statements must be indented.
- Use `==` to test for equality.
- `X and Y` is only true if both `X` and `Y` are true.
- `X or Y` is true if either `X` or `Y`, or both, are true.
- Zero, the empty string, and the empty list are considered false; all other numbers, strings, and lists are considered true.
- `True` and `False` represent truth values.

:::


