# Introduction to research computing on Picotte

This workshops teaches you how to use
[Picotte](https://docs.urcf.drexel.edu/clusters/picotte/), Drexel's
high-performance computing cluster.

You'll learn how Picotte is organized, how to connect and access Picotte, and
how to submit jobs to the scheduler.

## Preparation


```{admonition} Understand shell basics

This workshops assumes you know the basics of how to use a Unix shell. You can learn these in our [Introduction to the Unix shell](https://urcf.github.io/urcf_workshops/intro_linux/index.html) workshop.

```

```{admonition} Install VSCode and the "Remote - SSH" extension

We'll use [Visual Studio Code](https://code.visualstudio.com/) (VSCode) as our interface to Picotte. VSCode lets us edit files, transfer data, and run commands, all from a single application.

**1. Install VSCode**

Download and install VSCode from [code.visualstudio.com/download](https://code.visualstudio.com/download). Choose the installer for your operating system (Windows, macOS, or Linux) and follow the default installation steps.

**2. Install the Remote - SSH extension**

Open VSCode. Open the Extensions sidebar by pressing `Ctrl+Shift+X` (Windows/Linux) or `Cmd+Shift+X` (macOS). Search for **Remote - SSH** and click **Install** on the extension by Microsoft.


![Installing Remote - SSH extension](../fig/intro_picotte/intro/vscode_remote_ssh_extension.png)

**3. Ensure OpenSSH Client is installed**

VSCode's Remote - SSH extension requires an SSH client to be installed on your operating system. Typically, this is already installed, and there's nothing you need to do. However, if you run into connection problems, try the steps for your operating system below.

:::::{tab-set}
::::{tab-item} Windows

Open **Settings → Apps → Optional features** and look for **OpenSSH Client** in the list. If it's not there, click **Add a feature**, search for "OpenSSH Client", and install it.

::::
::::{tab-item} macOS

macOS includes a built-in SSH client, no additional setup is needed.

::::
::::{tab-item} Linux

Most Linux distributions include an SSH client by default. If not, install it with your package manager (e.g. `sudo apt install openssh-client` on Ubuntu/Debian).

::::
:::::

```


