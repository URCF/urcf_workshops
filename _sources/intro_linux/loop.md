# Loops

**Loops** are a programming construct which allow us to repeat a command
or set of commands for each item in a list.
As such they are key to productivity improvements through automation.
Similar to wildcards and tab completion, using loops also reduces the
amount of typing required (and hence reduces the number of typing mistakes).

## A motivating example

Let's say we want to count the number of female penguins on each island. We're working in the `analysis` directory, and we have separate files for each island: `biscoe_island.csv`, `dream_island.csv`, and `torgersen_island.csv`.

Let's make sure we're in the right directory:

```bash
$ cd ~/Downloads/shell-lesson-data/analysis
$ ls
biscoe_island.csv  data.csv  dream_island.csv  torgersen_island.csv
```

You might think we could use a wildcard to search all the island files at once:

```bash
$ grep female *_island.csv | wc -l
     165
```

But this doesn't work the way we want! This command searches all the files matching `*_island.csv` and outputs all the matching lines together, then counts the total. It's the same as if we'd done:

```bash
$ grep female data.csv | wc -l
     165
```

We get the total number of female penguins across all islands, but we don't know how many are on each individual island.

To count the female penguins on each island separately, we need to run the command once for each file. We could do this manually:

```bash
$ grep female biscoe_island.csv | wc -l
      80
$ grep female dream_island.csv | wc -l
      61
$ grep female torgersen_island.csv | wc -l
      24
```

But this is tedious and error-prone, especially if we had many more files. Instead, we can use a **loop** to automate this process.

## The general form of a loop

Here's the general form of a loop, using pseudo-code:

```bash
# The word "for" indicates the start of a "for loop"
for thing in list_of_things
#The word "do" indicates the start of the "loop body", which is the list of commands to be executed for each item in the list
do
    # Indentation within the loop is not required, but aids legibility
    command1 $thing
    command2 $thing
    ...
# The word "done" indicates the end of a loop
done
```

## Using a loop to count female penguins

Let's apply this to our problem of counting female penguins on each island:

```bash
$ for filename in *_island.csv
do
    echo $filename
    grep female $filename | wc -l
done
```

```output
biscoe_island.csv
      80
dream_island.csv
      61
torgersen_island.csv
      24
```

When the shell sees the keyword `for`, it knows to repeat a command (or group of commands) once for each item in a list. Each time the loop runs (called an iteration), an item in the list is assigned in sequence to the variable, and the commands inside the loop are executed, before moving on to the next item in the list. Inside the loop, we get the variable's value by putting `$` in front of it. The `$` tells the shell to treat something as a variable name and substitute its value in its place, rather than treat it as text or a command.

In this example, the list is three filenames: `biscoe_island.csv`, `dream_island.csv`, and `torgersen_island.csv`. Each time the loop iterates, we first use `echo` to print the value that the variable `$filename` currently holds. This is not necessary for the result, but helpful for us here to have an easier time to follow along. Next, we run the `grep` command on the file currently referred to by `$filename`, pipe it to `wc -l` to count the lines.

- The first time through the loop, `$filename` is `biscoe_island.csv`. The interpreter runs `grep female biscoe_island.csv | wc -l`, which counts the female penguins on Biscoe island (80).
- For the second iteration, `$filename` becomes `dream_island.csv`. This time, the shell runs `grep female dream_island.csv | wc -l`, which counts the female penguins on Dream island (61).
- For the third iteration, `$filename` becomes `torgersen_island.csv`, so the shell runs `grep female torgersen_island.csv | wc -l`, which counts the female penguins on Torgersen island (24).

Since the list was only three items, the shell exits the loop after this.

```{admonition} Challenge: Counting both female and male penguins
:class: tip

Write a loop that outputs both the number of female penguins and the number of male penguins on each island.

_Hint_: You can include as many commands as you like inside a loop (between `do` and `done`).

_Hint_: Remember from the pipes lesson that `grep male` will match both "male" and "female" (since "female" contains "male"), so you'll need to use `grep -v female` to exclude female penguins and count only the males.

:::{admonition} Solution
:class: dropdown

~~~bash
for filename in *island.csv
do
    echo $filename
    echo "- Female penguins:"
    grep female $filename | wc -l
    echo "- Male penguins:"
    grep -v female $filename | wc -l
done
~~~

This will output something like:

~~~output
biscoe_island.csv
- Female penguins:
      80
- Male penguins:
      83
dream_island.csv
- Female penguins:
      61
- Male penguins:
      62
torgersen_island.csv
- Female penguins:
      24
- Male penguins:
      23
~~~

:::
```
