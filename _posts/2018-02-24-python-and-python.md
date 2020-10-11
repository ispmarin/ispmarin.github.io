---
title: Python versions with Pyenv
categories: Python
tags:
    - pyenv
    - linux
    - python
date: 2018-02-24
---

The great easy of use of Python can be hindered by its own success. Python is a cornerstone of Linux, as many things
depend on a running Python interpreter. And that can lead, in a conservative distribution like Python, to very old 
Python versions. As of now, Debian Stretch, the stable version, has Python 2.7.13, that is not too bad with respect
to the latest stable version, 2.7.14, but it has 3.5.3, a long shot from 3.6.4. What to do now?

I tried to compile by hand and didn't have great success, until I came across this [post](https://jacobian.org/writing/python-environment-2018/).
It's all over the place with the tooling, but the suggestion of using [Pyenv](https://github.com/pyenv/pyenv) was great.

So let's install it.

## Github installations

I'm not a big fan of not using my distribution package manager, but it seems that everyone thinks that package managers
are a thing of the past and provide its own installation instructions. *sigh* Anyway, to install pyenv, the recommended
way is to clone the git repository:

```bash
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
``` 

and configure your shell to use it:

```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
PATH=$PATH:$PYENV_ROOT/bin
```

and enable the shims for autocompletion:

```bash
cho -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bashrc
```

Don't forget to reload your shell. 

## Installing a new Python
After installing pyenv, it's quite simple to install a new Python interpreter. The command

```bash
pyenv install 3.6.4
```

will download and compile Python 3.6.4. Quite handy. Make sure that you have `libssl-dev` and `libsqlite3-dev` installed. 

To select the just compiled Python, run

```bash
pyenv local 3.6.4
```
to use it. To revert to the system Python, run

```bash
pyenv local system
```

And that's it. Done!