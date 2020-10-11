---
title:  Basic Python Setup for Data Science on Linux
categories: data-science
date: 2017-11-20
tags:
  - data science
  - linux
  - python
---

Data scientists usually have a very diverse background and very different proficiency levels with some tools. This, as I usually say, is not negative, on the contrary! The best data science teams should have this diversity because the problems that they face are diverse by nature.
As I said before, I've been trying to set a system for data science with the tenets of reproducible research (RR).
To do that, not only the project should be organized to support RR, but the work environment should support RR too.

So the idea of this post is to explain some basic concepts about Linux, linux shells, environment variables and how to glue everything together on a productive Python environment.

## Linux and Linux shells

### Quick detour

First, to get this out of the way: I really like the open source software philosophy and the software that the community writes, but sometimes you have to give and see that some battles are best fought elsewhere. *Linux*, technically, is just the [kernel](https://en.wikipedia.org/wiki/Linux_kernel) of a [operating system](https://en.wikipedia.org/wiki/Operating_system), usually composed of free and open source software, called [distribution](https://en.wikipedia.org/wiki/Linux_distribution).
That's why sometimes you see "Debian GNU/Linux" distribution, because Debian, the Linux distribution, is composed of the [GNU](https://www.gnu.org/home.en.html) tools and the Linux kernel.
But the name "Linux" got very popular and usually means *both* the kernel and the distribution. So don't worry if you say only Linux. I don't.

### Shell, terminal, console

So, after this enormous rant, what is a shell? A [shell](https://en.wikipedia.org/wiki/Unix_shell) command line interpreter that works as a user interface.
A command line interpreter is exactly that, it interprets and executes the commands given into instructions to the operating system.
So shell is mostly just a program that reads commands typed into it! And best of all, it hides the details of the operating system.
Shells can be used in an interactive fashion, where users type the commands, or in an automated way via *scripts*. A script is a file with a series of commands to be executed.
To be more specific, there are the interactive login, interactive non-login and script shells.
Each has a different way to set environment variables and are called differently. The shell is also called *command line* or *terminal*.

### Running a command

Now let's see how a shell executes a command. Let's say we want to run `man ls`, to get the manual page of the command `ls`.
There are two ways for the shell to call the executable `man` with the argument `ls`.
The first one is to give the full path of the command to the shell:

```bash
>  /usr/bin/man
what manual page do you want?

> file /usr/bin/man
/usr/bin/man: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV),
dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2,
for GNU/Linux 2.6.32, BuildID[sha1]=ce062f3e946ea23a804345bc92b18983ab05c839, stripped
```

So the shell will execute the command located on the directory `/usr/bin/man`.
The other one is how the shell stores information for each session: using one specific *environment variable*.
In this case, the shell searches the `PATH` variable for the directories where it should seek the executable files.
If the shell finds on one of the dirs, the program is called. If not it returns an error:

```bash
> echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
```

So for our example, as the `man` command is located on the `/usr/bin` folder, calling only `man` on the shell will execute it:

```bash
> man
what manual page do you want?
```

Let's see now what environment variables are. With them we can understand a bit better how Python finds its packages.

### Environment Variables

Environment variables are ["a set of dynamic named values that can affect the way running processes will behave in a computer"](https://en.wikipedia.org/wiki/Environment_variable).
On a higher level, each program executed by the OS has its own set of environment variables.
When a shell is started, it reads a predefined set of files and define the environment variables for that shell session.
The files where the variables are defined and the order of the variable definitions depends on the shell and if the shell is interactive or script.
[This](https://shreevatsa.wordpress.com/2008/03/30/zshbash-startup-files-loading-order-bashrc-zshrc-etc/) has the order for [ZSH](http://www.zsh.org/) and [Bash](https://www.gnu.org/software/bash/) files that are *sourced*, defining the environment variables and executing the commands inside them.
(*Sourcing* a file executes the lines of code inside that file as they were typed on the shell.)

Several variables are common to all [Unix](https://help.ubuntu.com/community/EnvironmentVariables).
One of them is called `PATH` and it tells which directories should be searched for programs to be executed.
Another very common variable is called `HOME` and identifies the path to the home folder for each user.
To understand what that means more practically, we want to add a new folder in our `PATH` variable, so the shell can find the executable without us having to type the entire file path.
We are using `bash` and we want to change only the user path, so we edit the `.bashrc` file on his home folder, adding the line

```bash
export PATH=$PATH:/home/user/bin/
```

The `export` means to attribute to the variable `PATH` the value `/home/user/bin`.
The `$PATH` part means to use the existing value of the variable `PATH` and to append it with the path after it.
Now, every time a new shell is created for this user, this new folder will be added to the `PATH` variable, and the programs on this folder can be executed without having to type the entire file path.

## Python paths and libraries

One of the great advantages of using Python for data science is the vast array of libraries available.
This can be also a great pain when you have to manage different projects and different requirements.
Virtualenvs does that by manipulating some environment variables. See where this is going?
The idea is to understand how Python works with some directories and paths and manipulate them for the Python interpreter and the libraries, in a way where each project can have its own dependencies and module versions.

Python uses environment variables defined at compile time and before execution.
The list of [environment variables](https://docs.python.org/2/using/cmdline.html#environment-variables) defines some environment variables that can be changed before execution.
To check inside Python which variables are defined, run inside a Python shell

```bash
> python
Python 2.7.11+ (default, Feb 22 2016, 16:38:42)
[GCC 5.3.1 20160220] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> print sys.path
['', '/usr/lib/python2.7', '/usr/lib/python2.7/plat-x86_64-linux-gnu',
'/usr/lib/python2.7/lib-tk', '/usr/lib/python2.7/lib-old',
'/usr/lib/python2.7/lib-dynload', '/usr/local/lib/python2.7/dist-packages',
'/usr/lib/python2.7/dist-packages', '/usr/lib/python2.7/dist-packages/gtk-2.0',
'/usr/lib/pymodules/python2.7']
```

These are the paths where the Python interpreter will search for modules by default.

### Virtualenvs

[Virtualenvs](https://virtualenv.pypa.io/en/latest/) were created to isolate an environment, decoupling
the modules for each virtual environment and the system.
We're going to use them to create our development environments.
Virtualenv manipulates the paths and set to different directories than the standard ones.
Let's create a virtualenv using [virtualenvwrapper](https://virtualenvwrapper.readthedocs.org/en/latest/):

```bash
> mkvirtualenv test
Running virtualenv with interpreter /usr/bin/python2
New python executable in test/bin/python2
Also creating executable in test/bin/python
Installing setuptools, pip...done.
```

With that the virtualenv is created and the variables have been changed to reflect the local environment:

```bash
> python
Python 2.7.11+ (default, Feb 22 2016, 16:38:42)
[GCC 5.3.1 20160220] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.path
['', '/home/user/.virtualenvs/test/lib/python2.7',
'/home/user/.virtualenvs/test/lib/python2.7/plat-x86_64-linux-gnu',
'/home/user/.virtualenvs/test/lib/python2.7/lib-tk',
'/home/user/.virtualenvs/test/lib/python2.7/lib-old',
'/home/user/.virtualenvs/test/lib/python2.7/lib-dynload',
'/usr/lib/python2.7', '/usr/lib/python2.7/plat-x86_64-linux-gnu',
'/usr/lib/python2.7/lib-tk',
'/home/user/.virtualenvs/test/local/lib/python2.7/site-packages',
'/home/user/.virtualenvs/test/lib/python2.7/site-packages']
```

Notice now that new paths are added for packages to be searched and used, so new packages are installed on these folders.
The virtualenv directory can be changed, from the default `~/.virtualenvs` to any other folder where the user has access.

This structure allows to manage several different virtualenvs, each with its own packages and configurations.

### Changing the path to the Python executable

What if a new Python version should be compiled and used? Probably the system will have to keep its Python version,
as Linux distributions use Python for system operations.
One way to to it is to compile the needed Python version and put the path to the Python executable on the `PATH` variable. From there new virtualenvs can be created using this specific Python version.

### Project structure

To keep packages, versions and dependencies separated, each project should have its own virtualenv.
For each virtualenv the necessary packages are installed using [pip](https://pypi.python.org/pypi/pip).
The virtualenv directory should also be set to a known location, like `~/bin/virtualenvs`.
It's also good practice to keep a [requirements.txt](https://pip.pypa.io/en/stable/user_guide/) file on the git repository.

## Conclusion

That was a lot, for sure! Hopefully this will make clear how to use virtualenvs and environment variables to keep the
development and data analysis easier to manage, reproduce and deploy.
