---
title: "A walk in creating a new Python project"
categories: data-science
date: 2019-03-10
tags:
  - python
  - virtualenv
  - venv
---

Let's say that you're worried that your home internet is not working the way it should. Let's say that you think that the latency should be lower that it is, and that your shining new router that you just purchased it's not doing its job. What do you do? Use Netflix and if it works, it works? Download an Ubuntu ISO just for fun? No, of course not. You decide to create a Python package that will use the tool [mtr](http://www.bitwizard.nl/mtr/) to record the latency of a host, including an asynchronous module to be able to fire up to several hosts at the same time.

Well, this post is not the story of that tool, that is still being created - and hopefully not abandoned before it's done. No, this post is about the story of creating the environment to develop it. You could use the system Python (in my case, 3.5.3), but no, you need to use the latest Python, and create the virtualenv with the right options... Well, you don't need to do it, but let's admit, *it's fun*.

So the first step is to update [pyenv](https://github.com/pyenv/pyenv). As I installed Pyenv on my home folder, updating the pyenv definitions is just a `git pull` away:

```bash
cd $HOME/.pyenv
git pull
```

and the new Python versions will be there. Then, let's see what versions are those:

```bash
pyenv install --list
```
Ok, it seems that the latest Python, 3.7.2, is there! Let's install it:

```bash
pyenv install 3.7.2
```

And let's pyenv magic do its thing. After a few minutes, I have a new Python version compiled and ready to be used. Let's set as my default Python. Easy peasy:

```bash
pyenv local 3.7.2
```

So my new Python on my environment is the latest one. A weird quirk is that I have to exit this shell and create a new one to have access to it. Not sure why, probably something related to the crazyness that pyenv does with shims and whatnot. So, armed with my new Python, let's create a new virtualenv. But wait! There's something that I haven't checked, and could cause problems. My old virtualenvs are using the previous pyenv Python installed, 3.6. What happens with them, now that I changed the default Python for my user? 

```
> ls -l lib/venvs/ts/bin
python -> /home/isp/.pyenv/versions/3.6.6/bin/python
```

Hum, this works, but it's not ideal. Pyenv keeps the previous versions of Python when a new one is installed. But I have a big thing for reproducible environments, and even knowing that virtualenvs created with [venv](https://docs.python.org/3/library/venv.html) are [not](https://stackoverflow.com/questions/27186207/are-python-3-x-venv-environments-relocatable) [relocatable](https://stackoverflow.com/questions/22946719/python3-venv-can-env-directory-be-renamed) (how I wish they were!), messing with symbolic links of shims probably is not very solid on the long term. I want binaries there! Fortunately, there's an option on venv for that: `--copies`.

```bash
python -m venv --copies lib/venvs/mtr
```

and voilá:

```bash
ls -l lib/venvs/mtr/bin
python
```

is a binary, not a link. Mission accomplished. Now I can finally start working on the very interesting and not really necessary project.
