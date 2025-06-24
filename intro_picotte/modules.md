## Environment modules

If you are on the compute node, exit it. Once you get on the login node, type this:

~~~bash
srun --partition=def-sm --account=urcfprj --nodes=1 --cpus-per-task=2 --mem=4G --time=00:30:00 --pty /bin/bash -l
~~~

We have a lot of software installed on Picotte, but most of it is organized into *modules*, which
need to be loaded. To see which modules are available on Picotte, please type

~~~bash
module avail
~~~

Hit `SPACE` several times to get to the end of the module list. This is a very long list, and you can see that
there is a lot of software installed for you. If you want to see which versions of MATLAB are installed, you can type

~~~bash
module avail matlab
~~~

~~~bash
[lbn28@node047 ~]$ module avail matlab

------------------------------------------------------- /ifs/opt/modulefiles --------------------------------------------------------
   matlab/R2020b    matlab/R2021a    matlab/R2022b (D)

  Where:
   D:  Default Module

Module defaults are chosen based on Find First Rules due to Name/Version/Version modules found in the module tree.
See https://lmod.readthedocs.io/en/latest/060_locating.html for details.

Use "module spider" to find all possible modules and extensions.
Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".


[lbn28@node047 ~]$
~~~

Let's say you want to use R. To load the module, you will need to specify its full name.To see which versions
of R are available, type

~~~bash
[lbn28@node047 ~]$ module avail R/

------------------------------------------------------- /ifs/opt/modulefiles --------------------------------------------------------
   R/4.1.3          grinder/0.5.4    maeparser/gcc/1.2.4           mothur/1.44.3    piler/1.0
   R/4.2.2 (L,D)    ior/3.3.0        maeparser/intel/2020/1.2.4    mpfr/4.1.0       pilercr/1.06

------------------------------------------------------ /cm/shared/modulefiles -------------------------------------------------------
   cuda10.1/profiler/10.1.243    cuda11.1/profiler/11.1.1    intel/compiler/32/2019/19.0.5    intel/compiler/64/2020/19.1.3 (D)
   cuda10.2/profiler/10.2.89     cuda11.2/profiler/11.2.2    intel/compiler/32/2020/19.1.3    jupyter/12.3.0
   cuda11.0/profiler/11.0.3      cuda11.4/profiler/11.4.2    intel/compiler/64/2019/19.0.5    nvhpc-byo-compiler/21.2

  Where:
   L:  Module is loaded
   D:  Default Module

Module defaults are chosen based on Find First Rules due to Name/Version/Version modules found in the module tree.
See https://lmod.readthedocs.io/en/latest/060_locating.html for details.

Use "module spider" to find all possible modules and extensions.
Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".
~~~

This will give you a list of all modules which have the phrase "R/" in them (`module avail` is
not very sophisticated). Let's see what happens when you load the `R 4.2.2` module:

~~~bash
module load R/4.2.2
module list
~~~

R depends on other software to run, so we have configured the R module in a way that when
you load it, it automatically loads other modules that it depends on.

To start command-line R, you can simply type
~~~bash
R
~~~

To quit R, type
~~~bash
quit()
~~~