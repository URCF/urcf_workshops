# Accessing Picotte using JupyterHub

In addition to SSH using the command line, you can also access Picotte using
[JupyterHub](https://jupyter.org/hub).

JupyterHub gives you a web-based "notebook" interface for doing computation
using Picotte. Jupyter has its roots in the Python community, but works with
many other programming languages. It's great for interactive and exploratory
work, especially data visualization.

To use JupyterHub on Picotte, go to: https://jupyterhub.urcf.drexel.edu.


You will need to login with your Picotte username and password:


:::{figure} ../fig/intro_picotte/jupyter-1.png
JupyterHub log in
:::

Once you are logged, the following interface appears:

:::{figure} ../fig/intro_picotte/jupyter-2.png
JupyterHub interface
:::

This screen lets you configure the resources your JupyterHub session will need.
These are the same arguments we used before with `sbatch`, just shown in a
web-based GUI rather than on the command line.

We're going to leave them all at their defaults, except for **Account** and
**Partition**. Set these just as we have for all our other jobs:

| Parameter | Value |
|-----------|-------|
| Account   | workshopprj |
| Partition | def-sm |

This will launch the Jupyter notebook interface as a job on a compute node and
connect you to it in your browser. Then you can choose a kernel and start
coding:

:::{figure} ../fig/intro_picotte/jupyter-3.png
Jupyter Server interface
:::

You can find more details about using JupyterHub (for example, with other
programming languages and custom kernels) in the [official URCF
documentation](https://docs.urcf.drexel.edu/software/jupyterhub/).




