---
title: "I want a pony - and these Jupyter notebook features"
categories: data-science
date: 2019-02-24
tags:
  - chronicle
  - jupyter
  - python
  - pony
---

Let's say it upfront: I'm aware of how open source software is made, I try to contribute the way I can, and I can't, for several reasons, create a pull request with these features that I think would be awesome to have on a Jupyter notebook. Ok, with that out of the way, there are a few things that I believe would improve the way I use Jupyter notebooks.

Jupyter notebooks are, probably, my most used tool when working with EDAs. My first impulse is always to create a virtualenv, install Jupyter, get the data and off I go. But there are a few things that bother me everyday. A few are like paper cuts, very small but annoying; others are blockers depending on the context that I'm working. Let me try to explain the pony that I want.

This is also inspired by Joel Grus [talk](https://docs.google.com/presentation/d/1n2RlMdmv1p25Xy5thJUhkKGvjtV-dkAIsUXP-AL4ffI/edit) that I think highlights several problems in the data science workflow today.

## Reading notebooks outside Jupyter server - plaintext notebooks

This is probably the most problematic for me. I understand that most of the users of Jupyter notebooks work with them locally, on the same machine, or at most at a server one hop away. I have no such luxury: sometimes the data is *four* hops away from my machine, and I have to use arcane incantations and very complicated SSH tunnels to get to it, and most of the time I can't copy this data back to my machine. This has several consequences, and one of them is how slow things can get. So there are times that I just want to read quickly what is inside a notebook, without having to create a tmux session, source my virtualenv, start the Jupyter server with the right flags, open the right IP address - and this can be a challenge sometimes - copy and paste the authentication token, just to see one line of code or result. So a plain text notebook where I can read using Vim quickly and easily would be incredible.

There are also other points for a plain text notebook, that are well detailed elsewhere: easier to version control, easier to write, etcetera. Having a Markdown document that can be rendered and used as a Jupyter notebook, or a PDF, and edited as a plain text file would help me immensely. There is a [nice discussion](https://github.com/jupyter/notebook/issues/3694) over Jupyter github, but so far nothing mainline.

## Kernel definition inside the notebook

This is something that I think would increase notebook reproducibility. Jupyter kernels are defined in a different folder (that I have to search for every time, sometimes in my own blog) that defines several things that a notebook needs to run, like the interpreter and so on. This makes the notebook not very portable: the code in the notebook was written with one kernel in mind, and the other system may have a different kernel, and now you have to keep changing things, sometimes the kernel definition themselves, to get it working. So why not keep the kernel definition as a header on the plain text notebook? I know, this could cause problems server side, but again, for my scenario, having copied a notebook from a small server to one with more memory but a completely different PySpark installation, without knowing exactly what *it is*, being able to customize it per notebook would help me.

## Variable results and evaluation in Markdown text

This is also another one that I've seen people asking before: having a way to get the evaluated result of a function as a value in text would make writing reports directly on the same file much easier. The notebook would be truly one document, where the code could be omitted and the report printed, or both kept and rerun by whoever needed it. If this is associated with the plain text notebook and some way of evaluating it and using pandoc to generate a PDF, yeah... that would be awesome.

## Multiple languages on the same notebook

If the kernel definition per notebook was a reality, I think this one would be easier to implement - but remember, I know very little about how Jupyter is made. With this, multiple kernels could be defined on the header, and each code cell could have a tag indicating which kernel should be used. I leave to the reader to think how state would be kept and shared between cells with different kernels.

## Results in an external file

This one is more far fetched and I know that there are other ways to do this, but bear with me for a moment. Imagine that you could, with just a switch, get all results from the computing cells saved in a different file. Images, pandas dataframes, _print_ results, whatever: if it was not typed, it is saved on an external file. If you could specify the folder for this, version controlling the notebook, specially in the context of sensitive data, would be a major feature. Sometimes we want to keep the notebook code but not all results, for privacy issues, for example. Today the results are saved on the notebook file itself, making this workflow more complicated. I know this is achievable using only code, but as I said before, as we are talking about poneys...

## Conclusion

Before people start shouting "you can do that in Rmarkdown!", or something similar, like Zeppelin, I'm targeting this on Jupyter because it is *the* reference tool for EDAs and other parts of data science work. Hell, github and gitlab render notebooks on their page. And again, to reiterate my starting point, I appreciate immensely the work done by Jupyter developers, I use their tools daily, and would like to point again that this is not a complain or something that I think I'm entitled to. These are just things that I believe could help the way I have to do my work today.