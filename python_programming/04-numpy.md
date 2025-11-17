# Loading and analyzing data

Now that we've covered some Python basics and worked with a small amount of traffic data in a list, we'll learn how to work with the entire dataset—multiple days worth of traffic data. We're going to use professional-grade tools (NumPy) to analyze the entire dataset and answer bigger questions.

## Data Format

The data is stored in **comma-separated values** (CSV) format:

- Each **row** represents one full day.
- Each **column** represents an hour of the day (Hour 0 - 23).

The first few rows of our data file look like this:

```
598,369,312,367,835,2726,5689,6990,5985,5309,4603,4884,5104,5178,5501,5713,6292,6057,4907,3503,3037,2822,1992,1166
455,336,265,314,779,2571,5563,6676,5966,4832,4395,4411,4648,4602,5125,5502,5979,5663,4259,3069,2378,2030,1400,917
1335,769,570,413,399,740,1420,2220,3479,4546,4579,4438,4623,4440,4495,4483,4591,4477,4221,3672,3015,2769,2875,1931
```

Each number represents the number of vehicles that passed during that hour.

For example, the first value, `598`, means that 598 vehicles passed from midnight to 1 AM on the first day. The next value, `369`, means that 369 vehicles passed from 1 AM to 2 AM on the first day, etc.

## Loading data into Python

To begin processing the traffic data, we need to load it into Python. We can do that using a *library* called [NumPy](https://numpy.org/doc/stable), which stands for “Numerical Python”. Libraries are collections of Python code written by other people that you can install and use.

In general, you should use NumPy when you want to do fancy things with lots of numbers, especially if you have matrices or arrays. To tell Python that we'd like to start using NumPy, we need to **import** it[^installation]:

~~~python
import numpy
~~~

Importing a library is like getting a piece of lab equipment out of a storage locker and setting it up on the bench. Libraries provide additional functionality to Python, much like a new piece of equipment adds functionality to a lab space. Just like in the lab, importing too many libraries can sometimes complicate and slow down your programs - so we only import what we need for each program.

Once we've imported the library, we can ask the library to read our data file for us. The data is stored in a file called `traffic_data.txt`:

```python
numpy.loadtxt('traffic_data.txt', delimiter=',')
```

```output
array([[ 598.,  369.,  312.,  367.,  835., 2726., 5689., 6990., 5985.,
        5309., 4603., 4884., 5104., 5178., 5501., 5713., 6292., 6057.,
        4907., 3503., 3037., 2822., 1992., 1166.],
       [ 455.,  336.,  265.,  314.,  779., 2571., 5563., 6676., 5966.,
        4832., 4395., 4411., 4648., 4602., 5125., 5502., 5979., 5663.,
        4259., 3069., 2378., 2030., 1400.,  917.],
...
```

The expression `numpy.loadtxt(...)` is a **function call** that asks Python to run the **function** `loadtxt` of the `numpy` library. We use the dot notation (`.`) that we learned about with lists—here it tells Python to look inside the `numpy` library and find the `loadtxt` function. When you see `numpy.loadtxt`, you can read it as "the `loadtxt` function that belongs to `numpy`".

`numpy.loadtxt` has two **parameters**: the name of the file we want to read and the delimiter that separates values on a line. These both need to be strings, so we put them in quotes.

Since we haven't told it to do anything else with the function's output, the notebook just displays it. In this case, that output is the data we just loaded. By default, only a few rows and columns are shown (with `...` to omit elements when displaying big arrays). Note that, to save space when displaying NumPy arrays, Python does not show us trailing zeros, so `598.0` becomes `598.`.

Our call to `numpy.loadtxt` read our file but didn't save the data in memory. To do that, we need to assign the array to a variable. In a similar manner to how we assign a single value to a variable, we can also assign an array of values to a variable using the same syntax. Let's re-run `numpy.loadtxt` and save the returned data:

```python
traffic_data = numpy.loadtxt('traffic_data.txt', delimiter=',')
```

This statement doesn't produce any output because we've assigned the output to the variable `traffic_data`. If we want to check that the data have been loaded, we can print the variable's value:

```python
print(traffic_data)
```

```output
[[ 598.  369.  312.  367.  835. 2726. 5689. 6990. 5985. 5309. 4603. 4884.
  5104. 5178. 5501. 5713. 6292. 6057. 4907. 3503. 3037. 2822. 1992. 1166.]
 [ 455.  336.  265.  314.  779. 2571. 5563. 6676. 5966. 4832. 4395. 4411.
  4648. 4602. 5125. 5502. 5979. 5663. 4259. 3069. 2378. 2030. 1400.  917.]
 [1335.  769.  570.  413.  399.  740. 1420. 2220. 3479. 4546. 4579. 4438.
  4623. 4440. 4495. 4483. 4591. 4477. 4221. 3672. 3015. 2769. 2875. 1931.]
 [ 500.  324.  257.  354.  769. 2769. 5789. 7055. 6408. 5254. 4547. 4784.
  5015. 5057. 5430. 5813. 6496. 6429. 5142. 3311. 2777. 2331. 1767. 1093.]
 [ 654.  374.  301.  397.  794. 2564. 5358. 6441. 5536. 5256. 4619. 5107.
  5147. 5283. 5704. 5887. 6307. 5953. 4519. 3427. 2921. 2649. 2005. 1177.]
 [ 511.  346.  253.  349.  807. 2669. 5210. 6083. 5790. 5484. 4407. 4725.
  4826. 4945. 5065. 5618. 6094. 5844. 4649. 3180. 2963. 2450. 1613. 1021.]
 [1159.  822.  641.  411.  468.  723. 1415. 2191. 3185. 4076. 4522. 6012.
  5975. 5307. 4678. 4405. 4403. 4311. 4198. 3475. 3078. 2931. 2941. 2240.]
 [1564.  795.  447.  443.  330.  453.  755.  953. 1650. 2442. 3230. 3575.
  4139. 3892. 3989. 4294. 4521. 4280. 3945. 3169. 2772. 2213. 1570. 1065.]
 [1166.  724.  568.  343.  383.  703. 1229. 2057. 3246. 4390. 4573. 4269.
  4833. 4442. 4397. 4314. 4413. 4125. 4004. 3404. 2777. 2579. 2552. 1952.]
 [1090.  735.  537.  371.  397.  696. 1301. 1932. 2952. 3529. 3837. 4338.
  4762. 4367. 4369. 4536. 4520. 4677. 4025. 3128. 2905. 2853. 3288. 2239.]]
```

Now that the data are in memory, we can manipulate them. First, let's ask what **type** of thing `traffic_data` refers to:

```python
print(type(traffic_data))
```

```output
<class 'numpy.ndarray'>
```

The output tells us that `traffic_data` currently refers to an N-dimensional array, the functionality for which is provided by the NumPy library.

With the following command, we can see the array's **shape**:

```python
print(traffic_data.shape)
```

```output
(10, 24)
```

The output tells us that the `traffic_data` array variable contains 10 rows and 24 columns. These extras like `shape` are called **attributes** or **members**. This extra information describes `traffic_data` in the same way an adjective describes a noun. `traffic_data.shape` is an attribute of `traffic_data` which describes the dimensions of `traffic_data`. We use the same "dot" notation for the attributes of variables that we use for methods because they have the same part-and-whole relationship.

To get a single number from the array, we have to provide an index, just like we do with lists. Unlike with lists, however, our traffic data now has two dimensions (day and hour), so we will need to use two indices to refer to one specific value:

```python
print('first day, first hour:', traffic_data[0, 0])
```

```output
first day, first hour: 598.0
```

```python
print('traffic at 7 AM on the second day:', traffic_data[1, 7])
```

```output
traffic at 7 AM on the second day: 6676.0
```

The expression `traffic_data[1, 7]` accesses the element at row 1 (second day), column 7 (7 AM). Since we're working with a 2D array, we need two indices: the first for the row (day) and the second for the column (hour). Remember that Python uses zero-based indexing, just like we learned with lists.

!['traffic_data' is a 3 by 3 numpy array containing row 0: ['A', 'B', 'C'], row 1: ['D', 'E', 'F'], and row 2: ['G', 'H', 'I']. Starting in the upper left hand corner, traffic_data[0, 0] = 'A', traffic_data[0, 1] = 'B', traffic_data[0, 2] = 'C', traffic_data[1, 0] = 'D', traffic_data[1, 1] = 'E', traffic_data[1, 2] = 'F', traffic_data[2, 0] = 'G', traffic_data[2, 1] = 'H', and traffic_data[2, 2] = 'I', in the bottom right hand corner.](../fig/python_programming/02-numpy/python-zero-index.svg)

```{admonition} In the Corner
:class: note

What may also surprise you is that when Python displays an array, it shows the element with index `[0, 0]` in the upper left corner rather than the lower left. This is consistent with the way mathematicians draw matrices but different from the Cartesian coordinates. The indices are (row, column) instead of (column, row) for the same reason, which can be confusing when plotting data.

```

## Analyzing data

NumPy has several useful functions that take an array as input to perform operations on its values. Now we have all the data, we can ask bigger questions. What was the average traffic for each hour across all days? Which hour is the busiest on average? What was the single busiest hour ever recorded?

If we want to find the average traffic for all days and all hours, for example, we can ask NumPy to compute `traffic_data`'s mean value:

```python
print(numpy.mean(traffic_data))
```

```output
3209.195833333333
```

~~~{admonition} Not All Functions Have Input
:class: note

Generally, a function uses inputs to produce outputs. However, some functions produce outputs without needing any input. For example, checking the current time doesn't require any input.

```python
import time
print(time.ctime())
```

```output
Sat Mar 26 13:07:33 2016
```

For functions that don't take in any arguments, we still need parentheses (`()`) to tell Python to actually run the function (which is called *calling* or *executing* the function).

~~~

Let's use three other NumPy functions to get some descriptive values about the dataset. We'll also use multiple assignment, a convenient Python feature that will enable us to do this all in one line.

```python
maxval, minval, stdval = numpy.max(traffic_data), numpy.min(traffic_data), numpy.std(traffic_data)

print('maximum traffic:', maxval)
print('minimum traffic:', minval)
print('standard deviation:', stdval)
```

Here we've assigned the return value from `numpy.max(traffic_data)` to the variable `maxval`, the value from `numpy.min(traffic_data)` to `minval`, and so on.

```output
maximum traffic: 7055.0
minimum traffic: 253.0
standard deviation: 2034.23
```

```{admonition} Mystery Functions in Python
:class: tip

How did we know what functions NumPy has and how to use them? If you are working in a Jupyter Notebook, there is an easy way to find out. If you type the name of something followed by a dot, then you can use **tab completion** (e.g. type `numpy.` and then press <kbd>Tab</kbd>) to see a list of all functions and attributes that you can use. After selecting one, you can also add a question mark (e.g. `numpy.cumprod?`), and IPython will return an explanation of the method! This is the same as doing `help(numpy.cumprod)`. Similarly, if you are using the "plain vanilla" Python interpreter, you can type `numpy.` and press the <kbd>Tab</kbd> key twice for a listing of what is available. You can then use the `help()` function to see an explanation of the function you're interested in, for example: `help(numpy.cumprod)`.

```

When analyzing data, though, we often want to look at variations in statistical values, such as the maximum traffic per day or the average traffic per hour. We can use loops to iterate through each day and calculate statistics. For example, let's find the maximum traffic of the first day in the dataset:

```python
day_0 = traffic_data[0]
print('maximum traffic for day 0:', numpy.max(day_0))
```

```output
maximum traffic for day 0: 6990.0
```

We can also get the maximum traffic of day 2 (remember, this is the *third* day, as in Python we start counting from zero):

```python
day_2 = traffic_data[2]
print('maximum traffic for day 2:', numpy.max(day_2))
```

```output
maximum traffic for day 2: 4579.0
```

What if we need the maximum traffic for each day over all hours (as in the next diagram on the left) or the average for each hour (as in the diagram on the right)? As the diagram below shows, we want to perform the operation across an axis:

![Per-day maximum traffic is computed row-wise across all columns using numpy.max(traffic_data, axis=1). Per-hour average traffic is computed column-wise across all rows using numpy.mean(traffic_data, axis=0).](../fig/python_programming/02-numpy/python-operations-across-axes.png)

To support this functionality, most array functions allow us to specify the *axis* we want to work on. If we ask for the average across axis 0 (the rows), we get the average traffic for each hour across all days:

```python
print(numpy.mean(traffic_data, axis=0))
```

```output
[ 857.8  501.3  408.5  356.4  508.5 1960.9 4561.1 5710.5 5318.4 4802.1
 4388.5 4700.2 4807.3 4770.2 4990.1 5300.1 5500.2 5400.1 4560.1 3300.2
 2750.1 2560.2 2200.1 1500.2]
```

This is a *one-dimensional array*, where each entry corresponds to an hour, and represents the average traffic across all days for that hour.

If we average across axis 1 (columns), we get:

```python
print(numpy.mean(traffic_data, axis=1))
```

```output
[4109.75 3821.25 3080.25 4109.75 4109.75 3821.25 3080.25 3080.25 3080.25 3080.25]
```

which is the average traffic per day across all hours.

We can also find which hour is the busiest on average:

```python
hourly_averages = numpy.mean(traffic_data, axis=0)
busiest_hour = numpy.argmax(hourly_averages)
print('Busiest hour (0-23):', busiest_hour)
print('Average traffic at that hour:', hourly_averages[busiest_hour])
```

```output
Busiest hour (0-23): 7
Average traffic at that hour: 5710.5
```

And we can find the single busiest hour ever recorded:

```python
print('Single busiest hour ever:', numpy.max(traffic_data))
```

```output
Single busiest hour ever: 7055.0
```

~~~{admonition} Challenge: Finding Maximum Traffic Per Day
:class: note

Write a loop that finds and prints the maximum traffic for each day in the dataset. Use `numpy.max()` to find the maximum value for each day.

:::{dropdown} Solution

```python
for i in range(len(traffic_data)):
    day_max = numpy.max(traffic_data[i])
    print('Day', i, 'maximum traffic:', day_max)
```

This loops through each day (row) in the data and finds the maximum traffic value for that day.

:::

~~~

```{admonition} Keypoints
- Import a library into a program using `import libraryname`.
- Use the `numpy` library to work with arrays in Python.
- The expression `array.shape` gives the shape of an array.
- Use `array[x, y]` to select a single element from a 2D array.
- Array indices start at 0, not 1.
- Use `# some kind of explanation` to add comments to programs.
- Use `numpy.mean(array)`, `numpy.max(array)`, and `numpy.min(array)` to calculate simple statistics.
- Use `numpy.mean(array, axis=0)` or `numpy.mean(array, axis=1)` to calculate statistics across the specified axis.
```


[^installation]: Usually, we would have to first *install* NumPy before we could import it. Here we're using an environment that comes with NumPy already installed, so we don't need to.
