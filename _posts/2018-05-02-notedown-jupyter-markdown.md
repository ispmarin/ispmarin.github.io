---
title: Jupyter notebooks converted to Markdown - and back
categories: Python
tags:
    - jupyter
    - data science
    - reproducibility
date: 2018-05-02
---

The JSON format that the Jupyter notebooks are made of are not very git friendly. They are also not suited to be edited
on an external editor. In this aspect, RMarkdown is much better. But, Python being Python, there is a package that can
convert from notebooks to markdown and back: [notedown](https://github.com/aaren/notedown).
 One can convert from a notebook to a markdown file and vice versa from the command line. To install, just run

```
pip install notedown
```

To convert from ipynb to md, run

```
notedown my_jupyter_note.ipynb --to markdown --strip > my_jupyter_note.md
```

The strip parameter removes the output from the cells, making the file cleaner. To revert the process, run

```
notedown my_jupyter_note.md > my_jupyter_note_and_back.ipynb
```

and done. I tested it on a quite complex notebook and the back and forth process worked without problems.