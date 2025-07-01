# Running a batch job

Interactive jobs are great if you need to do something quick (perhaps visualize
some data). If you have code that runs for seven hours, an interactive job is
not a great idea. This is because an interactive job is killed if you close the
SSH connection. So, for example, if you start an interactive job, but then your
laptop falls asleep, the SSH connection will disconnect and your job will be
killed.

If you have some truly serious, multi-hour (or multi-day) computation (and
that's what Picotte is really good for), a better idea is to run it in the
background using a **batch job**. Submitting a batch job is conceptually similar
to an interactive job (just a different command), but the job will run on its
assigned compute node in the background until it's over. If it needs to take two
days, it takes two days. You can quit the SSH client or close your laptop, it
won't affect a batch job.

Let's walk through an example of running a batch job. This will put together
everything we've learned, from job submission, to using environment modules to
load scientific software on Picotte.

## Our first batch job: BLAST search

As an example to test Picotte's capabilities, we're going to run a DNA sequence
search using [BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi). BLAST (Basic
Local Alignment Search Tool) is a tool for searching DNA and RNA sequences
databases—you give it a sequence you're interested in (called a "query
sequence") and it searches databases of known sequences (for example, the human
genome) to find similar sequences to the query, using an algorithmic process
called "alignment". The details aren't important, don't worry if you don't
understand bioinformatics—we're just using it as an example.

### Download query sequence

We're going to search using the sequence of the
[BRCA1](https://en.wikipedia.org/wiki/BRCA1) gene as our query. BRCA1 is a
*tumor suppressor gene*, mutations in which can cause breast cancer. It's famous
as an early example of a specific gene that could be linked to cancer.

To start, download the gene sequence file by right clicking here:
[`BRCA1.fasta`](https://github.com/URCF/urcf_workshops/raw/master/data/BRCA1.fasta)
and choosing "Download" or "Save file".

Then, connect to Picotte with [Cyberduck](./storage.md) and upload the
`BRCA1.fasta` file by dragging and dropping it into the Cyberduck window. Make
sure you're putting the file in your home directory.

If you SSH into Picotte, you can confirm the file is there:

```
$ ls BRCA1.fasta
BRCA1.fasta
```

And check its contents to see that its a DNA sequence:

```
cat BRCA1.fasta
>BRCA1
AGCTCGCTGAGACTTCCTGGACCCCGCACCAGGCTGTGGGGTTTCTCAGATAACTGGGCCCCTGCGCTCAGGAGGCCTTC
ACCCTCTGCTCTGGGTAAAGTTCATTGGAACAGAAAGAAATGGATTTATCTGCTCTTCGCGTTGAAGAAGTACAAAATGT
CATTAATGCTATGCAGAAAATCTTAGAGTGTCCCATCTGTCTGGAGTTGATCAAGGAACCTGTCTCCACAAAGTGTGACC
ACATATTTTGCAAATTTTGCATGCTGAAACTTCTCAACCAGAAGAAAGGGCCTTCACAGTGTCCTTTATGTAAGAATGAT
ATAACCAAAAGGAGCCTACAAGAAAGTACGAGATTTAGTCAACTTGTTGAAGAGCTATTGAAAATCATTTGTGCTTTTCA
GCTTGACACAGGTTTGGAGTATGCAAACAGCTATAATTTTGCAAAAAAGGAAAATAACTCTCCTGAACATCTAAAAGATG
AAGTTTCTATCATCCAAAGTATGGGCTACAGAAACCGTGCCAAAAGACTTCTACAGAGTGAACCCGAAAATCCTTCCTTG
CAGGAAACCAGTCTCAGTGTCCAACTCTCTAACCTTGGAACTGTGAGAACTCTGAGGACAAAGCAGCGGATACAACCTCA
AAAGACGTCTGTCTACATTGAATTGGGATCTGATTCTTCTGAAGATACCGTTAATAAGGCAACTTATTGCAGTGTGGGAG
ATCAAGAATTGTTACAAATCACCCCTCAAGGAACCAGGGATGAAATCAGTTTGGATTCTGCAAAAAAGGCTGCTTGTGAA
...
```

If you can't get the transfer to work with Cyberduck, you can instead use the following command:

```
wget https://github.com/URCF/urcf_workshops/raw/master/data/BRCA1.fasta
```

To download the `BRCA1.fasta` file directly onto Picotte instead.

### Prepare SLURM script

To run a batch job, we need to write a **SLURM script**. This is file containing
a series of commands we want the job to run, along with configuration arguments,
like the account and partition, or requests for additional resources.

Making sure you're in your home directory on Picotte (you can get there with `cd
~`), type:

~~~bash
nano blast_job.sh
~~~

This will open the `nano` text editor with an empty file:

:::{figure} ../fig/intro_Picotte/nano_empty.png
Nano just opened with empty file.
:::

Inside the editor, type this:

~~~
#!/bin/bash

#SBATCH --account=workshopprj
#SBATCH --partition=def-sm

module load ncbi-blast/2.13.0

blastn -query BRCA1.fasta -db patnt
~~~

Instead of typing, you can copy the text from the Web browser and paste it into
`nano`. Windows users can paste with `Shift`+`Ins` (or by right-clicking the
mouse). Mac users can paste with `Cmd`+`V`. At the end, your screen should look
like this:

To save the script, press `Ctrl`+`o`, and then press `Enter`. `nano` will prompt
you to choose a name for the new file. Press `Enter` again to accept the default
name or `blast_job.sh`. To exit `nano`, press `Ctrl`+`x`. To make sure the text
is saved properly, print it on screen using the `cat` command:

~~~bash
cat blast_job.sh
~~~

What do these lines mean? Let's look at them one by one:

:::{card} 1.
```
#!/bin/bash
```

This somewhat cryptic line is called a
["shebang"](https://en.wikipedia.org/wiki/Shebang_(Unix)). The details are
beyond the scope of this workshop, but it tells the system that this script is
meant to be executed using the bash shell (as opposed to, say, a programming
language like `python` or `R`).
:::

:::{card} 2.
```
#SBATCH --account=workshopprj
```

This line passes an argument to the `sbatch`. In an `sbatch` script, any line
that starts with `#SBATCH` will be passed as arguments, just as though you had
typed it on the command line. This lets you save your arguments in your script,
so you don't have to type them out every time you run the job.

Here we say we want to submit this job using the `workshopprj` project. This is
just like when we submitted our interactive job.
:::

:::{card} 3.
```
#SBATCH --partition=def-sm
```

Similar to the above line. Here we specify the `def-sm` partition. Again, this
is just like when we specified the partition in our interactive job.
:::

:::{card} 4.
```
module load ncbi-blast/2.13.0
```

This should look more familiar. Here we're using [Environment
modules](./environment.md) to load version 2.13.0 of the BLAST command line
tools.
:::

:::{card} 5.
```
blastn -query BRCA1.fasta -db patnt
```

Finally, this line actually invokes the `blastn` command to run the BLAST
search. The `-query BRCA1.fasta` means we want to search using the contents of
`BRCA1.fasta` as our query sequence. The `-db` argument tells BLAST which
database we want to search. Here we're searching the `patnt` database, which is
a database of nucleotide (DNA or RNA) sequences that have appeared in patents.
:::

### Submitting the job

Now, let's submit our batch job! We do this using another SLURM command,
`sbatch`. `sbatch` takes a single argument, the name of the job script to
submit. Let's try it:

~~~
[jjp366@picotte001 ~]$ sbatch blast_job.sh
Submitted batch job 11755768
~~~

If the submission was successful, it will give you a **job ID**, as shown above.
You can use the job ID to monitor the job's progress and check its output.

### Monitoring the job

The `scontrol` command shows detailed information about a job's progress. To use
this command, run `scontrol show job <job id>`, replacing `<job id>` with the
job ID output by sbatch.

```
[jjp366@picotte001 ~]$ scontrol show job 11755768
JobId=11755768 JobName=blast_job.sh
   UserId=jjp366(2451) GroupId=jjp366(2462) MCS_label=N/A
   Priority=11927 Nice=0 Account=workshopprj QOS=normal WCKey=*
   JobState=RUNNING Reason=None Dependency=(null)
   Requeue=1 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:0
   RunTime=00:00:06 TimeLimit=00:30:00 TimeMin=N/A
   SubmitTime=2025-06-27T19:03:10 EligibleTime=2025-06-27T19:03:10
   AccrueTime=2025-06-27T19:03:10
   StartTime=2025-06-27T19:03:10 EndTime=2025-06-27T19:33:11 Deadline=N/A
   SuspendTime=None SecsPreSuspend=0 LastSchedEval=2025-06-27T19:03:10 Scheduler=Main
   Partition=def-sm AllocNode:Sid=picotte001:21706
   ReqNodeList=(null) ExcNodeList=(null)
   NodeList=node035
   BatchHost=node035
   NumNodes=1 NumCPUs=1 NumTasks=1 CPUs/Task=1 ReqB:S:C:T=0:0:*:*
   TRES=cpu=1,mem=4000M,node=1,billing=1
   Socks/Node=* NtasksPerN:B:S:C=0:0:*:* CoreSpec=*
   MinCPUsNode=1 MinMemoryCPU=4000M MinTmpDiskNode=0
   Features=(null) DelayBoot=00:00:00
   OverSubscribe=OK Contiguous=0 Licenses=(null) Network=(null)
   Command=/home/jjp366/blast_job.sh
   WorkDir=/home/jjp366
   StdErr=/home/jjp366/slurm-11755768.out
   StdIn=/dev/null
   StdOut=/home/jjp366/slurm-11755768.out
   Power=
```

This is a lot of details! Not all of these are relevant to us, but there are some useful things:

| What we can tell                           | How we know                                 |
|--------------------------------------------|---------------------------------------------|
| Job is running                             | `JobState=RUNNING`                          |
| Assigned to node035                        | `NodeList=node035`                          |
| Running for 6 seconds                      | `RunTime=00:00:06`                          |
| Output will be in `slurm-11755768.out`     | `StdOut=/home/jjp366/slurm-11755768.out`    |

----

If you don't want all these details, you can get a quick overview of the jobs
you have running using the `squeue` command and passing your username as the
`-u` option. For example:

```
[jjp366@picotte001 ~]$ squeue -u jjp366
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
          11755768    def-sm blast_jo   jjp366  R       0:17      1 node035
```

The `ST` column means "status" and `R` means the job is running. You might also
see `PD`, for "pending". This means your job is waiting for resources to become
available.

If you keep checking the status of the job with `scontrol`, you'll eventually
see `JobState=RUNNING` change to `JobState=COMPLETED` (usually within a minute
if your job didn't have to wait in the queue). This means the job is done!

Let's check the output with less:

```
less slurm-11755768.out
```

Remember to replace `11755768` with your job ID. You should see something like:

```
BLASTN 2.13.0+


Reference: Zheng Zhang, Scott Schwartz, Lukas Wagner, and Webb
Miller (2000), "A greedy algorithm for aligning DNA sequences", J
Comput Biol 2000; 7(1-2):203-14.



Database: Nucleotide sequences derived from the Patent division of GenBank
           46,121,564 sequences; 25,865,402,917 total letters



Query= BRCA1

Length=5914
                                                                      Score     E
Sequences producing significant alignments:                          (Bits)  Value

DL205917.1 17q-Linked Breast and Ovarian Cancer Susceptibility Gene   10922   0.0
DL212974.1 17q-Linked Breast and Ovarian Cancer Susceptibility Gene   10922   0.0
BD105583.1 Genes sensitive to 17q-chained breast cancer and ovari...  10922   0.0
AR136942.1 Sequence 1 from patent US 6162897                          10922   0.0
AR008159.1 Sequence 1 from patent US 5753441                          10922   0.0
AR004673.1 Sequence 1 from patent US 5747282                          10922   0.0
I81034.1 Sequence 1 from patent US 5710001                            10922   0.0
I80938.1 Sequence 1 from patent US 5709999                            10922   0.0
I76943.1 Sequence 1 from patent US 5693473                            10922   0.0
DJ442504.1 ORPHAN RECEPTOR TYROSINE KINASE AS A TARGET IN BREAST ...  10914   0.0
DI151791.1 CARCINOMA CELL-SPECIFIC APOPTOSIS INDUCING AGENT TARGE...  10914   0.0
DJ032279.1 BIOMARKERS OF CYCLIN-DEPENDENT KINASE MODULATION           10914   0.0
DD328314.1 Method for Predicting Autoimmune Disease                   10914   0.0
DD237544.1 Agent for inducing apoptosis in a cancer cell by inhib...  10914   0.0
MX626585.1 Sequence 3 from patent US 10989719                         10892   0.0
...
```

Unsurprisingly for BRCA1, most of the sequences we found are from patents about
cancer or cancer testing. Several of these patents (e.g. [Patent
5747282](https://patents.google.com/patent/US5747282A/en)) were the subject of a
Supreme Court case, [Association for Molecular Pathology v. Myriad Genetics,
Inc.](https://en.wikipedia.org/wiki/Association_for_Molecular_Pathology_v._Myriad_Genetics,_Inc.),
which decided that naturally occurring DNA sequences are not patentable.

### Requesting more resources

This job was pretty fast, but if we were searching a much bigger database, or
searching with many sequences, and the job was taking a long time? How could we
 speed it up?

A lot of research software can use multiple CPU cores to improve performance,
and BLAST is no exception. Let's try it by editing our job script with `nano`:

```
nano blast_job.sh
```

Add the following line next to the other `#SBATCH` lines:

```
#SBATCH --cpus-per-task=2
```

This tells SLURM that our job requires two CPU cores (the default is one).

However, this is not enough to improve the performance. Just because SLURM
allocates two cores, does not mean BLAST will automatically use them. We have
explicitly tell BLAST that we want it to use multiple cores.

Change the final `blastn` command to add the argument `-num_threads 2`:

```
blastn -query BRCA1.fasta -db patnt -num_threads 2
```

The file should now look like this:


```
#!/bin/bash

#SBATCH --account=workshopprj
#SBATCH --partition=def-sm
#SBATCH --cpus-per-task=2

module load ncbi-blast/2.13.0

blastn -query BRCA1.fasta -db patnt -num_threads 2
```

Like before, save the file using `Ctrl`+`o`, press `Enter` when prompted for the
filename, and quit `nano` using `Ctrl`+`x`.

----

We can now run the job again with `sbatch`:

```
[jjp366@picottemgmt ~]$ sbatch blast_job.sh
Submitted batch job 11755850
```

And track its progress:

```
[jjp366@picottemgmt ~]$ scontrol show job 11755850
JobId=11755850 JobName=blast_job.sh
   UserId=jjp366(2451) GroupId=jjp366(2462) MCS_label=N/A
   Priority=11927 Nice=0 Account=workshopprj QOS=normal WCKey=*
   JobState=RUNNING Reason=None Dependency=(null)
   Requeue=1 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:0
   RunTime=00:00:05 TimeLimit=00:30:00 TimeMin=N/A
   SubmitTime=2025-06-30T11:28:58 EligibleTime=2025-06-30T11:28:58
   AccrueTime=2025-06-30T11:28:58
   StartTime=2025-06-30T11:28:59 EndTime=2025-06-30T11:58:59 Deadline=N/A
   SuspendTime=None SecsPreSuspend=0 LastSchedEval=2025-06-30T11:28:59 Scheduler=Main
   Partition=def-sm AllocNode:Sid=picottemgmt:2653761
   ReqNodeList=(null) ExcNodeList=(null)
   NodeList=node039
   BatchHost=node039
   NumNodes=1 NumCPUs=2 NumTasks=1 CPUs/Task=2 ReqB:S:C:T=0:0:*:*
   TRES=cpu=2,mem=8000M,node=1,billing=2
   Socks/Node=* NtasksPerN:B:S:C=0:0:*:* CoreSpec=*
   MinCPUsNode=2 MinMemoryCPU=4000M MinTmpDiskNode=0
   Features=(null) DelayBoot=00:00:00
   OverSubscribe=OK Contiguous=0 Licenses=(null) Network=(null)
   Command=/home/jjp366/blast_job.sh
   WorkDir=/home/jjp366
   StdErr=/home/jjp366/slurm-11755850.out
   StdIn=/dev/null
   StdOut=/home/jjp366/slurm-11755850.out
   Power=
```

We can see that we've succesfully assigned two CPUS by `NumCPUs=2`.

The job should finish faster than when it ran with only one CPU[^speed].

### Other types of resources

In this example, we just requested more CPUs. Your jobs can request much more
than just that though: additional memory, multiple nodes, time limit, GPUs and
more can all be requested using arguments to `sbatch`.

The details are beyond the scope of this workshop, but for more see the [URCF
SLURM documentation](https://docs.urcf.drexel.edu/learning/slurm/reference/) and
the [official `sbatch` documentation](https://slurm.schedmd.com/sbatch.html).

[^speed]: This will be highly variable because a lot of the work of this job is
    loading the nucleotide database from disk, which can be faster or sloweer
    depending on cacheing, how many other people are accessing the database,
    etc. This is a valuable lesson: software performance is very hard to
    predict! Simply adding more cores won't always make things faster. You need
    to carefully profile your code to ensure you're optimizing the right thing.