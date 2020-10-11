---
title: Fancy setup for Deep Learning with Jupyter themes
categories: data-science
tags:
    - deep learning
    - jupyter notebook
    - python
date: 2018-01-28
---

I like color themes, so when I found out about [Jupyter Notebook themes](https://github.com/dunovank/jupyter-themes) I had
to try it. I also suffer from the "setup dependency hell" in my home notebook, as I don't use it frequently do to development.
Usually the cycle goes like this:

- start a shell and open iPython on it
- notices that a Python package is out of date, update it with `pip install -U jupyter`
- start the Jupyter notebook
- arghg, I haven't set up the password and now I have to copy that string
- Ok, let's do it properly, let's set up the password for the notebooks
- but wait, where is the folder where the config file goes? Darn it, let's google it
- ok, just need to generate the Jupyter notebook configuration file, easy
- but this is the thousand time that I've done it, let's document it
- fire up PyCharm to write about it
- PyCharm wants to update itself...

![this is crazy](https://media.giphy.com/media/D12CsrRNv7gL6/giphy.gif)

So I decided to write this post so at least part of the stuff up there is in the same place.


## Update all Python packages in a virtualenv using pip

First thing: let's update all packages in a virtualenv. First, activate the virtualenv and run this command:

```bash
pip freeze --local | grep -v '^\-e' | cut -d = -f 1 | xargs -n1 pip install -U
```

or put it in a script. I keep mine in `$HOME/bin/pip_update.sh`.

## Jupyter notebook folders

The []default folders for Jupyter](http://jupyter.readthedocs.io/en/latest/projects/jupyter-directories.html) are:

- `JUPYTER_CONFIG_DIR`: `$HOME/.jupyter`, the configuration folder
- `JUPYTER_PATH`: `$HOME/.local/share/jupyter`, the data folder. It also follows the `XDG_DATA_HOME` variable.

These paths can be found using the command

```bash
jupyter --paths
```

## Configuring Jupyter notebook password
I recommend setting up a password for your Jupyter notebooks, even if you'll use it locally. It is documented
[here](http://jupyter-notebook.readthedocs.io/en/stable/public_server.html). Create a configuration file with

```bash
jupyter notebook --generate-config
```

so, if the instructions above are correct, a file called `jupyter_notebook_config.py` is created at the folder
`$HOME/.jupyter`. We had in the past to hash a password by ourselves and configure it on the configuration file above,
but now there is a helper command that will do that for us. From the terminal, use

```bash
jupyter notebook password
```
And you will be prompted to give it a password. The password will be saved on `$HOME/.jupyter/jupyter_notebook_config.json`.

### Bonus: use SSL
This is maybe overkill for a local installation, but it's so easy to set up SSL for a notebook, so why not? Let's generate
a SSL certificate with

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout jupyter_notebook.key -out jupyter_notebook.pem
```
and save it in `$HOME/.jupyter`. Now, open the configuration file `jupyter_notebook_config.py` and fill up these sections:

```python
# Set options for certfile, ip, password, and toggle off
# browser auto-opening
c.NotebookApp.certfile = u'/home/user/.jupyter/jupyter_notebook.pem'
c.NotebookApp.keyfile = u'/home/user/.jupyter/jupyter_notebook.key'
# Set ip to '*' to bind on all interfaces (ips) for the public server
# c.NotebookApp.ip = '*'
c.NotebookApp.open_browser = False
```

## Notebook themes

I think that color themes can help both in the presentation aspect but also in the focus aspect. I found out the
[Jupyter notebook themes](https://github.com/dunovank/jupyter-themes) and so far I think it's great. It's quite easy to
install, just use `pip`:

```bash
pip install jupyterthemes
```

and select one with

``` bash
jt -t grade3
```
for the `grade3` theme. The list of available themes can be obtained by running `jt -l`. It's awesome.

## Finally, Keras with TensorFlow and CNTK

The installation procedure for Keras with TensorFlow backend got a bit easier, mostly because TF now is a bit better
behaved to install using `pip`. It's quite standard now:

```bash
pip install tensorflow
pip install keras
```

and that's it. The CNTK package is still not in Pypi, but it's also [easy to install](https://docs.microsoft.com/en-us/cognitive-toolkit/setup-linux-python?tabs=cntkpy231):

```bash
sudo apt-get install openmpi-bin
pip install https://cntk.ai/PythonWheel/CPU-Only/cntk-2.3.1-cp35-cp35m-linux_x86_64.whl
```

and done. Note that I'm installing everything with CPU support only, as to run with CUDA with my machine things get a bit more involved
(bumblebee and so forth). Done! So go and [run some examples](https://keras.io/getting-started/functional-api-guide/) now.
