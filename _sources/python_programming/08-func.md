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

# Creating Functions

:::{admonition} Objectives

- Define a function that takes parameters.
- Return a value from a function.
- Test and debug a function.
- Set default values for function parameters.
- Explain why we should divide programs into small, single-purpose functions.

:::

:::{admonition} Questions

- How can I define new functions?
- What's the difference between defining and calling a function?
- What happens when I call a function?

:::

At this point, we've seen that code can have Python make decisions about what it
sees in our data. What if we want to convert some of our data, like taking a
temperature in Fahrenheit and converting it to Celsius. We could write something
like this for converting a single number

```{code-cell} python
fahrenheit_val = 99
celsius_val = (fahrenheit_val - 32) * (5/9)
```

and for a second number we could just copy the line and rename the variables

```{code-cell} python
fahrenheit_val = 99
celsius_val = (fahrenheit_val - 32) * (5/9)

fahrenheit_val2 = 43
celsius_val2 = (fahrenheit_val2 - 32) * (5/9)
```

But we would be in trouble as soon as we had to do this more than a couple
times. Cutting and pasting it is going to make our code get very long and very
repetitive, very quickly. We'd like a way to package our code so that it is
easier to reuse, a shorthand way of re-executing longer pieces of code. In
Python we can use 'functions'. Let's start by defining a function
`fahr_to_celsius` that converts temperatures from Fahrenheit to Celsius:

```{code-cell} python
def fahr_to_celsius(temp):
    return (temp - 32) * (5/9)
```

Now we can use this function to convert temperatures:

```{code-cell} python
print(fahr_to_celsius(32))
```

![](../fig/python_programming/python-function.svg)

The function definition opens with the keyword `def` followed by the name of the
function (`fahr_to_celsius`) and a parenthesized list of parameter names
(`temp`). The [body](../learners/reference.md#body) of the function---the
statements that are executed when it runs---is indented below the definition
line.  The body concludes with a `return` keyword followed by the return value.

When we call the function,
the values we pass to it are assigned to those variables
so that we can use them inside the function.
Inside the function,
we use a [return statement](../learners/reference.md#return-statement) to send a result
back to whoever asked for it.

Calling our own function is no different from calling any other function (like
built-in functions, or functions from a library). For example, we can use it in
a print statement:

```{code-cell} python
print('freezing point of water:', fahr_to_celsius(32), 'C')
print('boiling point of water:', fahr_to_celsius(212), 'C')
```

We've successfully called the function that we defined, and we have access to
the value that we returned.

## Composing Functions

Now that we've seen how to turn Fahrenheit into Celsius,
we can also write the function to turn Celsius into Kelvin:

```{code-cell} python
def celsius_to_kelvin(temp_c):
    return temp_c + 273.15

print('freezing point of water in Kelvin:', celsius_to_kelvin(0.))
```

What about converting Fahrenheit to Kelvin?
We could write out the formula,
but we don't need to.
Instead,
we can [compose](../learners/reference.md#compose) the two functions we have already created:

```{code-cell} python
def fahr_to_kelvin(temp_f):
    temp_c = fahr_to_celsius(temp_f)
    temp_k = celsius_to_kelvin(temp_c)
    return temp_k

print('boiling point of water in Kelvin:', fahr_to_kelvin(212.0))
```

This is our first taste of how larger programs are built: we define basic
operations, then combine them in ever-larger chunks to get the effect we want.
Real-life functions will usually be larger than the ones shown here---typically
half a dozen to a few dozen lines.

~~~{admonition} Variable Scope
:class: tip

In composing our temperature conversion functions, we created variables inside of those functions,
`temp`, `temp_c`, `temp_f`, and `temp_k`.
We refer to these variables as [local variables](../learners/reference.md#local-variable)
because they no longer exist once the function is done executing.
If we try to access their values outside of the function, we will encounter an error:

```python
print('Again, temperature in Kelvin was:', temp_k)
```

```error
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-1-eed2471d229b> in <module>
----> 1 print('Again, temperature in Kelvin was:', temp_k)

NameError: name 'temp_k' is not defined
```

If you want to reuse the temperature in Kelvin after you have calculated it with `fahr_to_kelvin`,
you can store the result of the function call in a variable:

```python
temp_kelvin = fahr_to_kelvin(212.0)
print('temperature in Kelvin was:', temp_kelvin)
```

```output
temperature in Kelvin was: 373.15
```

The variable `temp_kelvin`, being defined outside any function,
is said to be [global](../learners/reference.md#global-variable).

Inside a function, one can read the value of such global variables:

```python
def print_temperatures():
  print('temperature in Fahrenheit was:', temp_fahr)
  print('temperature in Kelvin was:', temp_kelvin)

temp_fahr = 212.0
temp_kelvin = fahr_to_kelvin(temp_fahr)

print_temperatures()
```

```output
temperature in Fahrenheit was: 212.0
temperature in Kelvin was: 373.15
```

~~~

## Tidying up

Now that we know how to wrap bits of code up in functions,
we can make our traffic data analysis easier to read and easier to reuse.

First, let's make sure we have our traffic data loaded:

```{code-cell} python
import numpy
import matplotlib.pyplot as plt

traffic_data = numpy.loadtxt('traffic_data.txt', delimiter=',')
```

Let's break out the logic we used before for determining if a day is a weekday into its own function:

```{code-cell} python
def is_weekday(day_data):
    return day_data[7] > 5000
```

This function takes a single day's data and returns `True` if it's a weekday (has heavy morning traffic at 7am) or `False` if it's a weekend. Now we can use this function in our plotting code:

```{code-cell} python
for day in traffic_data:
    if is_weekday(day):
        plt.plot(day, color='blue')
    else:
        plt.plot(day, color='orange')
plt.title('Traffic Patterns: Weekdays vs Weekends')
plt.xlabel('Time of Day')
plt.ylabel('Vehicle Count');
```

Notice how much clearer the `if` statement is now! Instead of `if data[i, 7] > 5000:`, we have `if is_weekday(data[i]):`. The function name `is_weekday` makes the intent of the condition immediately clear—we're checking whether this day is a weekday. This makes the code more readable because anyone reading it can understand *what* we're checking for, not just *how* we're checking it.Even better, if we need to check if a day is a weekday elsewhere in our code, we can reuse this function. Then we don't have to write the same code over and over again, or update it in multiple places if we want to change it.

## Readable functions

Consider these two functions:

```{code-cell} python
def s(p):
    a = 0
    for v in p:
        a += v
    m = a / len(p)
    d = 0
    for v in p:
        d += (v - m) * (v - m)
    return numpy.sqrt(d / (len(p) - 1))

def std_dev(sample):
    sample_sum = 0
    for value in sample:
        sample_sum += value

    sample_mean = sample_sum / len(sample)

    sum_squared_devs = 0
    for value in sample:
        sum_squared_devs += (value - sample_mean) * (value - sample_mean)

    return numpy.sqrt(sum_squared_devs / (len(sample) - 1))
```

The functions `s` and `std_dev` are computationally equivalent (they
both calculate the sample standard deviation), but to a human reader,
they look very different. You probably found `std_dev` much easier to
read and understand than `s`.

As this example illustrates, both documentation and a programmer's
*coding style* combine to determine how easy it is for others to read
and understand the programmer's code. Choosing meaningful variable
names and using blank spaces to break the code into logical "chunks"
are helpful techniques for producing *readable code*. This is useful
not only for sharing code with others, but also for the original
programmer. If you need to revisit code that you wrote months ago and
haven't thought about since then, you will appreciate the value of
readable code!

~~~{admonition} Challenge 1: Combining Strings
:class: note

"Adding" two strings produces their concatenation:
`'a' + 'b'` is `'ab'`.
Write a function called `fence` that takes two parameters called `original` and `wrapper`
and returns a new string that has the wrapper character at the beginning and end of the original.
A call to your function should look like this:

```python
print(fence('name', '*'))
```

```output
*name*
```

:::{dropdown} Solution
```python
def fence(original, wrapper):
    return wrapper + original + wrapper
```
:::
~~~

~~~{admonition} Challenge 2: Return versus print
:class: note

Note that `return` and `print` are not interchangeable.
`print` is a Python function that *prints* data to the screen.
It enables us, *users*, see the data.
`return` statement, on the other hand, makes data visible to the program.
Let's have a look at the following function:

```python
def add(a, b):
    print(a + b)
```

**Question**: What will we see if we execute the following commands?

```python
A = add(7, 3)
print(A)
```

:::{dropdown} Solution
Python will first execute the function `add` with `a = 7` and `b = 3`,
and, therefore, print `10`. However, because function `add` does not have a
line that starts with `return` (no `return` "statement"), it will, by default, return
nothing which, in Python world, is called `None`. Therefore, `A` will be assigned to `None`
and the last line (`print(A)`) will print `None`. As a result, we will see:

```output
10
None
```
:::
~~~

~~~{admonition} Challenge 3: Selecting Characters From Strings
:class: note

If the variable `s` refers to a string,
then `s[0]` is the string's first character
and `s[-1]` is its last.
Write a function called `outer`
that returns a string made up of just the first and last characters of its input.
A call to your function should look like this:

```python
print(outer('helium'))
```

```output
hm
```

:::{dropdown} Solution
```python
def outer(input_string):
    return input_string[0] + input_string[-1]
```
:::
~~~

~~~{admonition} Challenge 4: Rescaling an Array
:class: note

Write a function `rescale` that takes an array as input
and returns a corresponding array of values scaled to lie in the range 0.0 to 1.0.
(Hint: If `L` and `H` are the lowest and highest values in the original array,
then the replacement for a value `v` should be `(v-L) / (H-L)`.)

:::{dropdown} Solution
```python
def rescale(input_array):
    L = numpy.min(input_array)
    H = numpy.max(input_array)
    output_array = (input_array - L) / (H - L)
    return output_array
```
:::
~~~

~~~{admonition} Challenge 5: Testing and Documenting Your Function
:class: note

Run the commands `help(numpy.arange)` and `help(numpy.linspace)`
to see how to use these functions to generate regularly-spaced values,
then use those values to test your `rescale` function.
Once you've successfully tested your function,
add a docstring that explains what it does.

:::{dropdown} Solution
```python
"""Takes an array as input, and returns a corresponding array scaled so
that 0 corresponds to the minimum and 1 to the maximum value of the input array.

Examples:
>>> rescale(numpy.arange(10.0))
array([ 0.        ,  0.11111111,  0.22222222,  0.33333333,  0.44444444,
       0.55555556,  0.66666667,  0.77777778,  0.88888889,  1.        ])
>>> rescale(numpy.linspace(0, 100, 5))
array([ 0.  ,  0.25,  0.5 ,  0.75,  1.  ])
"""
```
:::
~~~

~~~{admonition} Challenge 6: Defining Defaults
:class: note

Rewrite the `rescale` function so that it scales data to lie between `0.0` and `1.0` by default,
but will allow the caller to specify lower and upper bounds if they want.
Compare your implementation to your neighbor's:
do the two functions always behave the same way?

:::{dropdown} Solution
```python
def rescale(input_array, low_val=0.0, high_val=1.0):
    """rescales input array values to lie between low_val and high_val"""
    L = numpy.min(input_array)
    H = numpy.max(input_array)
    intermed_array = (input_array - L) / (H - L)
    output_array = intermed_array * (high_val - low_val) + low_val
    return output_array
```
:::
~~~

~~~{admonition} Challenge 7: Variables Inside and Outside Functions
:class: note

What does the following piece of code display when run --- and why?

```python
f = 0
k = 0

def f2k(f):
    k = ((f - 32) * (5.0 / 9.0)) + 273.15
    return k

print(f2k(8))
print(f2k(41))
print(f2k(32))

print(k)
```

:::{dropdown} Solution
```output
259.81666666666666
278.15
273.15
0
```

`k` is 0 because the `k` inside the function `f2k` doesn't know
about the `k` defined outside the function. When the `f2k` function is called,
it creates a [local variable](../learners/reference.md#local-variable)
`k`. The function does not return any values
and does not alter `k` outside of its local copy.
Therefore the original value of `k` remains unchanged.
Beware that a local `k` is created because `f2k` internal statements
*affect* a new value to it. If `k` was only `read`, it would simply retrieve the
global `k` value.
:::
~~~

~~~{admonition} Challenge 8: Mixing Default and Non-Default Parameters
:class: note

Given the following code:

```python
def numbers(one, two=2, three, four=4):
    n = str(one) + str(two) + str(three) + str(four)
    return n

print(numbers(1, three=3))
```

what do you expect will be printed?  What is actually printed?
What rule do you think Python is following?

1. `1234`
2. `one2three4`
3. `1239`
4. `SyntaxError`

Given that, what does the following piece of code display when run?

```python
def func(a, b=3, c=6):
    print('a: ', a, 'b: ', b, 'c:', c)

func(-1, 2)
```

1. `a: b: 3 c: 6`
2. `a: -1 b: 3 c: 6`
3. `a: -1 b: 2 c: 6`
4. `a: b: -1 c: 2`

:::{dropdown} Solution
Attempting to define the `numbers` function results in `4. SyntaxError`.
The defined parameters `two` and `four` are given default values. Because
`one` and `three` are not given default values, they are required to be
included as arguments when the function is called and must be placed
before any parameters that have default values in the function definition.

The given call to `func` displays `a: -1 b: 2 c: 6`. -1 is assigned to
the first parameter `a`, 2 is assigned to the next parameter `b`, and `c` is
not passed a value, so it uses its default value 6.
:::
~~~

~~~{admonition} Readable Code
:class: note

Revise a function you wrote for one of the previous exercises to try to make
the code more readable. Then, collaborate with one of your neighbors
to critique each other's functions and discuss how your function implementations
could be further improved to make them more readable.
~~~



:::{admonition} Keypoints

- Define a function using `def function_name(parameter)`.
- The body of a function must be indented.
- Call a function using `function_name(value)`.
- Numbers are stored as integers or floating-point numbers.
- Variables defined within a function can only be seen and used within the body of the function.
- Variables created outside of any function are called global variables.
- Within a function, we can access global variables.
- Variables created within a function override global variables if their names match.
- Use `help(thing)` to view help for something.
- Put docstrings in functions to provide help for that function.
- Specify default values for parameters when defining a function using `name=value` in the parameter list.
- Parameters can be passed by matching based on name, by position, or by omitting them (in which case the default value is used).
- Put code whose parameters change frequently in a function, then call it with different parameter values to customize its behavior.

:::


