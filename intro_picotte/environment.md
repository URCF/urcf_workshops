# Environment modules

 We mentioned earlier that Picotte has a lot of research software pre-installed.
 How do you use it? The answer is a system called **environment modules**.

 Imagine we want to use [BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) (Basic
 Local Alignment Search Tool), which is installed on Picotte. BLAST is a tool
 for searching DNA and RNA sequence databases—you give it a query sequence and
 it finds similar sequences in databases (for example, the human genome) using
 a process called alignment. We'll use it again when we run a batch job later.

 Normally, on a system where BLAST is installed, you could run a command like
 `blastn -version` to check that it's available. However, if we try that on
 Picotte we get:

 ```
[jjp366@picotte001 ~]$ blastn -version
-bash: blastn: command not found
 ```

 This is because BLAST, like most software on Picotte, is not installed
 globally and available by default, but has to first be loaded using an
 environment module. To work with environment modules, use the `module`
 command.

 To see all the modules available for loading, run:

 ```
module avail
 ```

You can use the `SPACE` key to scroll through this output. As you can see,
there's a lot of software installed on Picotte. This is a little overwhelming,
so to narrow our search, we can run:

```
module avail ncbi-blast
```

This gives us more manageable output:

```
---------------------------------------------------------------------------------------------------------------------------- /ifs/opt/modulefiles ----------------------------------------------------------------------------------------------------------------------------
   ncbi-blast/2.11.0    ncbi-blast/2.13.0 (D)

  Where:
   D:  Default Module

Module defaults are chosen based on Find First Rules due to Name/Version/Version modules found in the module tree.
See https://lmod.readthedocs.io/en/latest/060_locating.html for details.

Use "module spider" to find all possible modules and extensions.
Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".
```

To load a module, we specify its full name, including the version number. Let's
say we want to use BLAST version 2.13.0. The command for this is:

```
module load ncbi-blast/2.13.0
```

This command gives no output, but BLAST has now been "loaded" into our shell
environment and is ready for use. Now if we try `blastn -version`:

```
[jjp366@picotte001 ~]$ blastn -version
blastn: 2.13.0+
Package: ncbi-blast 2.13.0
```

It works! BLAST is loaded and ready to use, and we see that the version matches
what we requested in our `module load` command.

## Why do we use modulefiles?

Why is it like this? Why not just have all software installed on Picotte
immediately available?

There's a hint above, in the form of the version numbers. Imagine you're working
on a project that uses BLAST 2.13, but another research group's project requires
BLAST 2.11, because version 2.13 removed support for a data format they use.

If there was a single, global BLAST installation, it would have to be one
version or the other, so one of you would be out of luck.

Environment modules solve this problem, and a number of similar
problems[^problems] by isolating software packages from each other, and letting
you only load the ones you need.

On Picotte, environment modules are managed by a tool called
[Lmod](https://lmod.readthedocs.io/en/latest/index.html). You can read the Lmod
documentation for much more detail about how it works and other options for the
`module` command.

[^problems]: For example, two pieces of software that conflict with each other,
    perhaps because they use the same name for their commands.