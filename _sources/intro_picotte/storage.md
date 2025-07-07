# Data storage and transfer

To run an actually useful compute job, you'll need to get your data in and out
of Picotte.

This section covers how and where to do that.

## Storage on Picotte

Picotte has two main storage locations, your home directory, or your research
group's shared directory.

:::{admonition} Home directory
Your home directory is similar to your home directory on your personal computer: a place for
you to keep your personal data, scripts, etc.

Everyone with a Picotte account has a home directory, located at
`/home/{username}`. So if your username is `bn23`, your home directory is
`/home/bn23` (You can also use the shortcut `~` to refer to your home
directory).

Your home directory is limited to 64GiB of data.

Your home directory is where you are when you log in.
:::

:::{admonition} Research group directory

Very similar to your home directory, except that it's shared between all members
of your research group. Research group directories are located in `/ifs/groups`.
For example, if you're part of Dr. Einstein's research group, your group
directory would be `/ifs/groups/einsteinGrp`.

Each group gets 500GiB of storage for free, and can pay for more storage beyond
this limit[^payment].

If you need to store more than 64GiB of data, or have data that other members of
your group need to access, keep it in your research group directory.
:::

The most important thing to know about both storage locations is that they
**shared between all Picotte nodes**. This means if you create files in your
home or group directory on the login node, they'll be available on all compute
nodes also, and vice-versa.

The way this works is that data is actually stored in a separate
computer[^isilon], and accessed by the login and compute over the network when
you read or write data. This is called *network-attached storage*.

:::{figure} ../fig/intro_picotte/storage/storage.png
:name: picotte-storage
:alt: Diagram of Picotte with shared storage shown

Shared storage on Picotte: nodes read and write data over the network
:::

This is very useful: you can write code and scripts on the login node, and run
them on the compute nodes without having to copy your code to the compute nodes.
When your jobs running on the compute nodes produce results, you can easily
examine them on the login node.

### Scratch space

Picotte also has **scratch space** for storing temporary files as part of a
job's execution. This is a more advanced topic than we'll cover in this
workshop, but you can read more in [our
documentation](https://docs.urcf.drexel.edu/clusters/picotte/scratch/).


## Transferring data to and from Picotte

### CyberDuck

There are many ways to transfer files between your local computer and Picotte. One piece of
software that works for both Mac and Windows machines is called CyberDuck. You can download
it [here](https://cyberduck.io/download/).

After installation, click on "Open Connection". A new window will pop up:

:::{figure} ../fig/intro_picotte/cyberduck_1.png
Cyberduck connection settings to Picotte
:::

To configure the connection for Picotte:
- In the drop-down menu on top, select "SFTP" instead of the default "FTP";
- In the "Server" input, specify `picottelogin.urcf.drexel.edu`;
- Make sure that "Port" is set to 22;
- Specify your Picotte username and password.
- Make sure "Save Password" or "Add to keychain" is checked.

Then, click on "Connect". If it complains about an "unknown fingerprint", click "Allow":

:::{figure} ../fig/intro_Picotte/cyberduck_2.png
Cyberduck connection settings to Picotte
:::

At this point, another new window will pop up, which contains the contents of
your Picotte home directory (if this is your first time using Picotte, it will
be empty). You can go to any other directory on Picotte by changing the path
(e.g. your research group directory). You can upload files by clicking the
"Upload" button, and download files by right-clicking them and selecting
"Download" (these might be under the "Actions" menu). You can also drag-and-drop
files to and from Picotte using this window.

:::{figure} ../fig/intro_Picotte/cyberduck_2fa.png
Home directory contents
:::

## Command line (scp)

Another option for advanced Mac and Linux users is the `scp` command. Open a new
terminal, but **don't connect to Picotte**. The `scp` command works like this:

~~~bash
scp <path_to_source> username@picottelogin.urcf.drexel.edu:<path_to_destination>
~~~

For example, here is the `scp` command to copy a file from the current directory on my local machine
to my home directory on Picotte (`lbn28` is my Picotte username):

~~~bash
scp myfile.txt username@picottelogin.urcf.drexel.edu:/home/lbn28/
~~~

... and to do the same in reverse, i.e., copy from Picotte to my local machine:

~~~bash
scp lbn28@picottelogin.urcf.drexel.edu:/home/lbn28/myfile.txt .
~~~

The `.` represents the working directory on the local machine.

To copy entire folders, include the `-r` switch:

~~~bash
scp -r myfolder lbn28@picottelogin.urcf.drexel.edu:/home/lbn28/
~~~


[^payment]: See [our
    documentation](https://docs.urcf.drexel.edu/clusters/picotte/usage-rates/#costs)
    for more on Picotte storage costs.
[^isilon]: In actuality, the network attached storage service is provided not
    just by a single computer, but a 12-node cluster dedicated to this task. We
    use a system from Dell called *Isilon*, so you might also hear or read the
    word "Isilon" used to refer to the shared storage on Picotte.