# Working with files and directories

We now know how to explore files and directories, but how do we create them?

Let's change into the `shell-lesson-data` directory that you downloaded earlier:

```
$ cd ~/Downloads/shell-lesson-data/
```

```{admonition} Downloading the example data
:class: note
If you didn't already download the example data, click [here](https://github.com/URCF/urcf_workshops/raw/master/data/shell-lesson-data.zip), then unzip the resulting zip file, leaving it in your Downloads folder.
```

Let's see what it contains:

```
$ ls
alkanes      creatures    numbers.txt  penguins.csv writing
```

There are a few files and directories. For now we're going to work with the `penguins.csv` file.

## Viewing a file

How can we see what's in `penguins.csv`?

The simplest way is a command called `cat`. `cat` takes the name of a file as
it's argument, and prints the contents of that file:

```
$ cat penguins.csv
```

This should print something like:

```
Chinstrap,Dream,42.5,16.7,187,3350,female,2008
Chinstrap,Dream,51,18.8,203,4100,male,2008
Chinstrap,Dream,49.7,18.6,195,3600,male,2008
Chinstrap,Dream,47.5,16.8,199,3900,female,2008
Chinstrap,Dream,47.6,18.3,195,3850,female,2008
Chinstrap,Dream,52,20.7,210,4800,male,2008
Chinstrap,Dream,46.9,16.6,192,2700,female,2008
Chinstrap,Dream,53.5,19.9,205,4500,male,2008
Chinstrap,Dream,49,19.5,210,3950,male,2008
Chinstrap,Dream,46.2,17.5,187,3650,female,2008
Chinstrap,Dream,50.9,19.1,196,3550,male,2008
Chinstrap,Dream,45.5,17,196,3500,female,2008
Chinstrap,Dream,50.9,17.9,196,3675,female,2009
Chinstrap,Dream,50.8,18.5,201,4450,male,2009
Chinstrap,Dream,50.1,17.9,190,3400,female,2009
Chinstrap,Dream,49,19.6,212,4300,male,2009
Chinstrap,Dream,51.5,18.7,187,3250,male,2009
Chinstrap,Dream,49.8,17.3,198,3675,female,2009
Chinstrap,Dream,48.1,16.4,199,3325,female,2009
Chinstrap,Dream,51.4,19,201,3950,male,2009
Chinstrap,Dream,45.7,17.3,193,3600,female,2009
Chinstrap,Dream,50.7,19.7,203,4050,male,2009
Chinstrap,Dream,42.5,17.3,187,3350,female,2009
Chinstrap,Dream,52.2,18.8,197,3450,male,2009
Chinstrap,Dream,45.2,16.6,191,3250,female,2009
Chinstrap,Dream,49.3,19.9,203,4050,male,2009
Chinstrap,Dream,50.2,18.8,202,3800,male,2009
Chinstrap,Dream,45.6,19.4,194,3525,female,2009
Chinstrap,Dream,51.9,19.5,206,3950,male,2009
Chinstrap,Dream,46.8,16.5,189,3650,female,2009
Chinstrap,Dream,45.7,17,195,3650,female,2009
Chinstrap,Dream,55.8,19.8,207,4000,male,2009
Chinstrap,Dream,43.5,18.1,202,3400,female,2009
Chinstrap,Dream,49.6,18.2,193,3775,male,2009
Chinstrap,Dream,50.8,19,210,4100,male,2009
Chinstrap,Dream,50.2,18.7,198,3775,female,2009
$
```

You can see this file is pretty big! There's so many lines that they don't all fit on the screen at once.

We can see just the first few lines using the `head` command. `head` accepts a `-n` option to specify the number of lines to show (if you don't pass `-n`, it defaults to 10 lines). Let's look at the first 5 lines:

```
$ head -n 5 penguins.csv
species,island,bill_length_mm,bill_depth_mm,flipper_length_mm,body_mass_g,sex,year
Adelie,Torgersen,39.1,18.7,181,3750,male,2007
Adelie,Torgersen,39.5,17.4,186,3800,female,2007
Adelie,Torgersen,40.3,18,195,3250,female,2007
Adelie,Torgersen,36.7,19.3,193,3450,female,2007
```

We can see the first line is the column headers, and the rest are the data.

The most convenient way to view long files like this is to use what's called a
"pager", which lets you scroll through the file one "page" at a time. The most
common pager is `less`, so named because it let's you see less of the file at once.

```
$ less penguins.csv
```

Now you're in the pager view. Use `↑` and `↓` to move
line-by-line, or try `b` and `Spacebar` to skip up and down by a full page. To
quit and return to the shell prompt, press `q`.

## Understanding the penguins data

The dataset we're looking at is called the ["Palmer Penguins"
dataset](https://allisonhorst.github.io/palmerpenguins/). It contains data on penguins
penguins sampled from three islands in the Palmer Archipelago, Antarctica between 2007 and 2009.

This format is called "comma-separated values", or CSV for short (hence
the `.csv` extension). Each column is a different variable, and each row is a
different penguin. CSV is a common format for tabular data, and can be exported
and imported by many programs, especially spreadsheet software like Excel.

<div style="display: flex; flex-direction: row; gap: 20px; justify-content: center; align-items: flex-start;">

<div style="flex: 1; text-align: center;">

```{figure} ../fig/intro_linux/file_system/penguin_species.png
---
name: penguin-species-fig
---
The three penguin species in the  dataset.
```

</div>

<div style="flex: 1; text-align: center;">

```{figure} ../fig/intro_linux/file_system/penguin_bill.png
---
name: penguin-bill-fig
---
Penguin bill measurements.
```

</div>
</div>

## Copying files

We're going to do some analysis on the penguins data. Let's make a copy of it, in case we mess something up and want to go back to the original later.

To do this, we use the `cp` command. `cp` takes two arguments: the source file and the destination file.

```
cp penguins.csv penguins2.csv
```

`cp` gives no output when it succeeds, but we can use `ls` to confirm that a copy of the file has been created:

```
$ ls
alkanes      analysis     creatures    numbers.txt  penguins.csv penguins2.csv writing
```

## Creating directories

Let’s also make a new directory to store our analysis results.

```
$ mkdir analysis
```

`mkdir` means "make directory".

```
$ ls
alkanes      analysis     creatures    numbers.txt  penguins.csv writing
```

Since we just created it, it has nothing in it, which we can confirm using `ls`. So far, we've only used `ls` with no arguments, but `ls` can take an argument: the name of a different directory to list. By default, it lists the current working directory, but we can give it any other directory instead. Let's list the contents of the `analysis` directory:

```
$ ls analysis
$
```

We can see that there's nothing there.

```{admonition} Challenge: mkdir and good names for directories
:class: tip

What do you think will happen if you run this command?

~~~
mkdir my directory
~~~

Think about it, decide what you think the result will be, then run the command.

What happened? Why do you think this is the case?

:::{admonition} Solution
:class: dropdown
This command creates *two* separate directories: `my` and `directory`:

~~~
$ mkdir my directory
$ ls
alkanes      analysis     creatures    directory    my           numbers.txt  penguins.csv writing
~~~

You might have expected it to create a single directory called "my directory". It creates two because in the shell, spaces are used to separate multiple arguments. `mkdir` can create multiple directories at once if you pass it multiple arguments.

This teaches us a valuable lesson. When working with the shell, it's best to **avoid spaces in names of directories and files**. Because of the special meaning of space as the   argument separator, file and directory names with spaces can be difficult and confusing in the shell. Use characters like `-` or `_` instead.

:::
```

## Moving and renaming files

Now let's move the copy of the penguins data to our new analysis directory. To do this, we use the `mv` command (short for "move"). `mv` is very similar to `cp`, but it moves the file instead of copying it.

 Just like `cp` `mv` takes two arguments: the source file and the destination. When the destination is a directory, `mv` will move the file into that directory.

```
$ mv penguins2.csv analysis/
```


Let's confirm that the file has been moved. First, we'll list the contents of the current directory:

```
$ ls
alkanes      analysis     creatures    numbers.txt  penguins.csv writing
```

We see that `penguins2.csv` is no longer there.

Now, let's change into our analysis directory and list its contents:

```
$ cd analysis
$ ls
penguins2.csv
```

We see that `penguins2.csv` is now in the `analysis` directory, and is the only file here.

The name `penguins2.csv` is a bit silly now that we're in a different directory, so there's no name conflicts. Let's change the name of the file to just `data.csv`. We also use the `mv` command for this. If you think of the full path to a file as it's location, re-naming is just moving it to a different path, so it uses the same command:

```
$ mv penguins2.csv data.csv
$ ls
data.csv
```

## Removing

Let’s go back to the `shell-lesson-data` directory and clean up a bit:

```
$ cd ..
```

We're only going to work with the penguin data today, so we don't need the other files. We can delete them using the `rm` (short for "remove") command.

For example, to delete the `numbers.txt` file, we can use:

```
$ rm numbers.txt
```

We can confirm the file is gone using `ls`:

```
$ ls
alkanes      analysis     creatures    penguins.csv writing
```

If we try to remove the `alkanes` directory using `rm alkanes`, we get an error message:

```
$ rm alkanes
rm: cannot remove 'alkanes': Is a directory
```

This happens because `rm` by default only works on files, not directories.

`rm` can remove a directory *and all its contents* if we use the recursive
option `-r`, and it will do so *without any confirmation prompts*:

```
$ rm -r alkanes
```

`rm -r` should be use with great caution, because it can permanently delete a
lot of files without asking for confirmation if you make a mistake.

```{admonition} Deleting is forever
:class: danger
Files deleted with `rm` cannot be recovered from a "trash bin" like files deleted using a GUI. Be very careful when deleting files using `rm`.
```

We can use `ls` again to confirm that the directory is gone:

```
$ ls
analysis     creatures    penguins.csv writing
```

<!-- TODO add challenges -->