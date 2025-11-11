# Redirection, wildcards, and pipes

Now that we know a few basic commands, we can finally look at the shell's most
powerful feature: the ease with which it lets us combine existing programs in new ways.

We'll keep working in our `analysis` directory inside `shell-lesson-data`.
`cd` there if you aren't there already:


```bash
$ cd ~/Downloads/shell-lesson-data/analysis
$ ls
data.csv
```

## Redirecting output

Now we want to analyze `data.csv`, looking at only one island at a time. There are three islands in the dataset: Torgersen, Biscoe, and     Dream. To start, let's look at only the penguins from Biscoe island.

We can use the `grep` command to filter the dataset to only include lines that contain a particular word. The syntax for this command is `grep <pattern> <file>`. The pattern is the word we want to search for, and the file is the file we want to search in.

To see only the lines that contain the word "Biscoe", we can run the following command:

```bash
$ grep Biscoe data.csv
```

The output includes only penguins from Biscoe island, because only these lines for these penguins contain the word "Biscoe".

```
...
Gentoo,Biscoe,48.8,16.2,222,6000,male,2009
Gentoo,Biscoe,47.2,13.7,214,4925,female,2009
Gentoo,Biscoe,46.8,14.3,215,4850,female,2009
Gentoo,Biscoe,50.4,15.7,222,5750,male,2009
Gentoo,Biscoe,45.2,14.8,212,5200,female,2009
Gentoo,Biscoe,49.9,16.1,213,5400,male,2009
```

There is a lot of output though! Let's save it to a file to make it easier to work with. We can use the `>` operator to redirect the output of the `grep` command to a file:

```
$ grep Biscoe data.csv > biscoe_island.csv
```

`>` "redirects" the output of a command to a file instead of printing it to the screen. If the file doesn't exist, it will be created. If it already exists, it will be overwritten (this can overwrite existing data, so be careful!).

Now we can use less to scroll through just the Biscoe penguins:

```
$ less biscoe_island.csv
```

Let's do the same for the other islands:

```
$ grep Torgersen data.csv > torgersen_island.csv
$ grep Dream data.csv > dream_island.csv
```

## Wildcards

Now we want to count the number of penguins in each island. To start, let's just count the total number of penguins in the dataset. We can use the `wc` command (short for "word count"), with the `-l` option, which says to count the number of lines in a file.

```
$ wc -l data.csv
     334 data.csv
```

This means there are 334 lines in the file, and each line represents a penguin, so there are 333 penguins (because the first line is the column headers).

Now let's count the number of penguins on Biscoe island:

```
$ wc -l biscoe_island.csv
     163 biscoe_island.csv
```

Now we could type out the commands for the other two islands one at at a time. And that would work because there are only three islands, but what if there were 100? It would be a pain to type out all those commands. Instead, we can use a shell feature called "wildcards" to tell `wc` to count the lines in all our files at once:

```
$ wc -l *.csv
     163 biscoe_island.csv
     334 data.csv
     123 dream_island.csv
      47 torgersen_island.csv
     667 total
```

The `*` is called a "wildcard". It matches zero or more characters of any type, so `*.csv` means "any file that ends with `.csv`". In this case, that's all our data files: `data.csv`, `biscoe_island.csv`, `dream_island.csv`, and `torgersen_island.csv`.

Now we can quickly see that there are `123` penguins on Dream island and `47` on Torgersen island. And this command would work just as well if there were 100 islands, or 1000! Now we're starting to see the power and flexibility of the shell.

```{admonition} Challenge: Count only the individual islands
:class: tip

You want to count the number of penguins in each island file, but *not* include `data.csv` (which also ends in `.csv`). How would you modify the command to only include the individual island files in your count, and exclude `data.csv`?

_Hint_: In the previous command, the wildcard `*.csv` matched all files ending in `.csv`. Do all your island files have something in common at the end of their filename that `data.csv` doesn't have?

:::{admonition} Solution
:class: dropdown

~~~bash
wc -l *island.csv
~~~

:::
```

## Passing output to another command

Now we want to count *only the female* penguins on each island. Let's start with Biscoe island.

We can use the `grep` command again to filter the data to only include female penguins by searching for the word "female":

```
$ grep female biscoe_island.csv
```

```
...
Gentoo,Biscoe,44.5,14.7,214,4850,female,2009
Gentoo,Biscoe,46.9,14.6,222,4875,female,2009
Gentoo,Biscoe,48.4,14.4,203,4625,female,2009
Gentoo,Biscoe,48.5,15,219,4850,female,2009
Gentoo,Biscoe,47.2,15.5,215,4975,female,2009
Gentoo,Biscoe,41.7,14.7,210,4700,female,2009
Gentoo,Biscoe,43.3,14,208,4575,female,2009
Gentoo,Biscoe,50.5,15.2,216,5000,female,2009
Gentoo,Biscoe,43.5,15.2,213,4650,female,2009
Gentoo,Biscoe,46.2,14.1,217,4375,female,2009
Gentoo,Biscoe,47.2,13.7,214,4925,female,2009
Gentoo,Biscoe,46.8,14.3,215,4850,female,2009
Gentoo,Biscoe,45.2,14.8,212,5200,female,2009
```

This is still a lot of output. We could use the `>` operator again to redirect it to a file, but if we did this every every island it would start to get unwieldy. Imagine we want to start filtering by year, or body weight. We would end up with a lot of extra files to keep track of.

It would be more convenient if we could somehow pass the output of the `grep` command directly to the `wc` command, rather than saving it to a file first. Fortunately, the shell has just such a feature: **pipes**. Pipes are a way to connect the output of one command as the input to another command. The syntax for a pipe is `command1 | command2`. This means "run `command1` and use it's output as the input of command2". `|` is called the "pipe character".

Let's try it out:

```
$ grep female biscoe_island.csv | wc -l
      80
```

So we can see there were 80 female penguins observed on Biscoe island.

The above command is equivalent to:
```
$ grep female biscoe_island.csv > temporary_file.csv
$ wc -l temporary_file.csv
$ rm temporary_file.csv
      80
```

It's just much shorter and more convenient. The pipe connects what's called the "standard output" of the first command to the "standard input" of the second command. Many Unix commands (like `wc`) can take input either from a file (which is what we've been doing so far) or from standard input (which is what we just did with the pipe).

### More complex filtering

Now let's say we want to find out how many female penguins were observed on Biscoe island in 2009. To do this, we can use a pipeline with multiple grep commands. Just like `wc`, `grep` can take it's input from a file or from standard input. So we'll `grep` from the file to filter for only female penguins, and then pipe the output to another `grep` to filter for only penguins observed in 2009, then finally pipe to `wc` to count them:

```
$ grep female biscoe_island.csv | grep 2009 | wc -l
      28
```



```{admonition} Challenge: Counting male penguins
:class: tip

Now we want to count the number of *male* penguins on Biscoe island. You might think we can do `grep male biscoe_island.csv`, but this returns **all** the penguins, not just the males! That's because the word "female" contains the letters "male" inside it, so `grep male` matches both "male" and "female". How can we count only the male penguins?

_Hint_: `grep` has a `-v` option (short for "i**v**ert"), which tells it to output all the lines that **don't** match the pattern you give it.

:::{admonition} Solution
:class: dropdown

~~~bash
grep -v female biscoe_island.csv | wc -l
~~~

:::
```

```{admonition} Challenge: Doing it all in one command
:class: tip

In all our pipelines so far, we've started from the `biscoe_island.csv` file. What if we wanted to do all the filtering in one command, starting with the original `data.csv`, without saving any intermediate file?

Can you write a single command, starting from `data.csv`, to count the female penguins on Biscoe island in 2009?

:::{admonition} Solution
:class: dropdown

~~~bash
grep female data.csv | grep Biscoe | grep 2009 | wc -l
~~~

:::
```

## Tools designed to work together

This idea of linking programs together is part of why Unix has been so
successful. Instead of a few enormous programs that try to do many different
things, Unix programmers has lots of simple tools that each do one job well, and
that work well with each other.

Almost all of the standard Unix tools can work this way. Unless told to do
otherwise, they read from standard input, do something with what they've read,
and write to standard output.

The key is that any program that reads lines of text from standard input and
writes lines of text to standard output can be combined with every other program
that behaves this way as well. You can and should write your programs this way
so that you and other people can put those programs into pipes to multiply their
power.