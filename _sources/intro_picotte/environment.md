# Environment modules

 We mentioned earlier that Picotte has a lot of research software pre-installed.
 How do you use it? The answer is a system called **environment modules**.

 Imagine we want to use, [Julia](https://julialang.org/), a programming language
 for scientific computing, which is installed on Picotte.

 Normally, on a system where Julia is installed, you just type:

 ```
julia
 ```

 at the prompt to start it. However, if we try that on Picotte we get:

 ```
[jjp366@picotte001 ~]$ julia
-bash: julia: command not found
 ```

 This is because Julia, like most software on Picotte, is not installed globally
and available by default, but has to first be loaded using an environment
 module. To work with environment modules, use the `module` command.

 To see all the modules available for loading, run:

 ```
module avail
 ```

You can use the `SPACE` key to scroll through this output. As you can see,
there's a lot of software installed on Picotte. This is a little overwhelming,
so to narrow our search, we can run:

```
module avail julia
```

This gives us more manageable output:

```
--------------------------------------------- /ifs/opt/modulefiles ---------------------------------------------
   julia/1.5.2    julia/1.6.0    julia/1.6.7    julia/1.7.3    julia/1.8.3    julia/1.8.5    julia/1.10.0 (D)
   julia/1.5.3    julia/1.6.5    julia/1.7.1    julia/1.8.2    julia/1.8.4    julia/1.9.1

  Where:
   D:  Default Module

Module defaults are chosen based on Find First Rules due to Name/Version/Version modules found in the module tree.
See https://lmod.readthedocs.io/en/latest/060_locating.html for details.

Use "module spider" to find all possible modules and extensions.
Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".
```

To load a module, we specify it's full name, including the version number. Let's
say we want to use Julia version 1.10.0. The command for this is:

```
module load julia/1.10.0
```

This command gives no output, but Julia has now been "loaded" into our shell
environment and is ready for use. Now if we try the `julia` command:

```
[jjp366@picotte001 ~]$ julia
               _
   _       _ _(_)_     |  Documentation: https://docs.julialang.org
  (_)     | (_) (_)    |
   _ _   _| |_  __ _   |  Type "?" for help, "]?" for Pkg help.
  | | | | | | |/ _` |  |
  | | |_| | | | (_| |  |  Version 1.10.0 (2023-12-25)
 _/ |\__'_|_|_|\__'_|  |  Official https://julialang.org/ release
|__/                   |

julia>
```

We get the result we expect, being dropped into an interactive shell.

We can return to the shell by typing `exit()` and pressing `Enter`:

## Why do we use modulefiles?

Why is it like this? Why not just have all software installed on Picotte
immediately available?

There's a hint above, in the form of the version numbers. Imagine you're working
on a project that uses Julia version 1.10, but another research group's code
Julia version 1.04.

If there was a single, globally Julia installation, it would have to be one
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