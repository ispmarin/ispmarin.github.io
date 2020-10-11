---
title: Secure Jupyter Notebook configuration
categories: Python
tags:
    - jupyter
    - data science
    - reproducibility
date: 2018-06-13
---

The first thing that I do after installing my Python environment is to configure Jupyter Lab to run under HTTPS. This
is important for [several reasons](https://developers.google.com/web/fundamentals/security/encrypt-in-transit/why-https).

To get started, first we will generate the SSL certificates:

```
openssl req -x509 -newkey rsa:4096 -keyout jupyter_key.key -out jupyter_cert.pem -days 365 -nodes
```

and answer the questions. Two files will be generated: `jupyter_key.key` and `jupyter_cert.pem`. These are your key and
certificate.

Now, we configure Jupyter to use these certificates. First create a Jupyter config file with

```
jupyter notebook --generate-config
```

This command will create a file `jupyter_notebook_config.py` at `$HOME/.jupyter`. Edit this file and add the lines:

```
c.NotebookApp.certfile = "/home/user/.jupyter/certs/jupyter_cert.pem"
c.NotebookApp.keyfile = "/home/user/.jupyter/certs/jupyter_key.key"
c.NotebookApp.open_browser = False
```

where we created the folder `certs` under `.jupyter` and copied the keys to there. After that, start Jupyter Notebook or
Jupyter Lab.

Remember that these certificates are self-signed, so your browser is going to emit a warning saying that they cannot be
trusted. Accept these certificates and check that your Jupyter session is protected by HTTPS.