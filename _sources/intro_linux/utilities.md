# Shell scripts

Now we're are going to take some commands we need to use frequently and save
them in a file so that we can re-run them later by typing a single command. A
bunch of commands saved in a file like this is called a **shell script**

Using shell scripts saves time, reduces errors, and makes your work more
reproducible.

## Using a text editor

Before we can create our first shell script, we need to learn how to use a text
editor to create and edit files. We'll use `nano`, which is a simple text editor
that works well in the shell.

To create or edit a file with `nano`, we use the command `nano <filename>`. For
example, to create a file called `example.txt`, we would run:

```bash
$ nano example.txt
```

This opens the `nano` editor. You'll see the editor interface with a header bar
at the top showing the filename. You can type text directly into the editor.
Let's type `Hello world!`.

To save the file, press `Ctrl+O` (press the `Ctrl` or `Control` key and, while
holding it down, press the `O` key). You'll be asked to   provide a name for the
file. Press `Return` to accept the suggested default filename (which is
`example.txt`, since we passed it as an argument to `nano` when we started it).

To quit the editor and return to the shell, press `Ctrl+X`.

Now let's confirm that the file was created and contains the text we typed:

```bash
$ cat example.txt
Hello world!
```

```{admonition} Common nano shortcuts
:class: note

- `Ctrl+O`: Save (Write Out) the file
- `Ctrl+X`: Exit nano
- `Ctrl+G`: Get Help (shows all available commands)
- `Ctrl+K`: Cut (delete) the current line
- `Ctrl+U`: Uncut (paste) the last cut line
```

## Our first shell script

Let's start by making sure we're in the `analysis` directory.

```bash
$ cd ~/Downloads/shell-lesson-data/analysis

```

Let's say we're all the time running pipelines of commands to count the number
of penguins observed in a specific year. We could do this by typing the command
out each time, but that would be tedious if we needed to do it a lot. Instead,
we can save the command to a file so we can re-run it later by typing a single
command. Let's create a file called `count_penguins.sh` and edit it with `nano`:

```
$ nano count_penguins.sh
```

In the `nano` editor, type the following command:

```bash
grep 2007 data.csv | wc -l
```

Note that we're *not* actually executing the command yet, we're just saving it to a file, which we can then execute later.

Save the file (`Ctrl+O` in nano) and exit (`Ctrl+X` in nano). Check that the directory now contains a file called `count_penguins.sh`:

```
cat count_penguins.sh
grep 2007 data.csv | wc -l
```

Once we have saved the file, we can ask the shell to execute the commands it contains. Our shell is called `bash`, so we run the following command:

```bash
$ bash count_penguins.sh
     103
```

This output is exactly what we would get if we ran that pipeline directly.

## Script arguments

What if we want to count penguins from a different year? We could edit `count_penguins.sh` each time to change the year, but that would be tedious if we needed to do it a lot. Instead, we can edit `count_penguins.sh` so it takes a year as an argument (much like the built-in commands we've been using take arguments to customize their behavior).

```bash
$ nano count_penguins.sh
```

Now, within "nano", replace the text `2007` with the special variable called `$1`:

```bash
grep "$1" data.csv | wc -l
```

Inside a shell script, `$1` means "the first argument passed on the command line". We can now run our script like this:

```bash
$ bash count_penguins.sh 2007
```

When the script executes, the `"$1"` is replaced with the argument, which in this case is `2007`.

We can try a different year like this:

```bash
$ bash count_penguins.sh 2009
```

## Executable script

So far, we've been running our scripts with `bash count_penguins.sh` to say that `bash` is the program that should execute the script. We can use a convenient shortcut to add this information to the script file itself, so we don't have to type it every time.

Edit the `count_penguins.sh` file to be as follows:

```source
#!/bin/bash
grep "$1" data.csv | wc -l
```

The first line `#!/bin/bash` is called a "shebang". It tells the system which program to use to execute the script.

To make the shell file executable, we need to change the permission on the file:

```bash
$ chmod u+x count_penguins.sh
```

`chmod` is a program used to change the permissions of a file. The `u+x` part means "make this file executable by the user who owns it".

Now we can run the script directly:

```bash
$ ./count_penguins.sh 2007
```

The `./` tells the shell to look for the script in the current directory. Without it, the shell would look for `count_penguins.sh` in the directories listed in your `PATH` environment variable, and wouldn't find it.
