---
title: Distributing Python modules for development
tags: python
categories: dev
---

Sometimes you write or (more likely to be my case) modify already existing python modules to suit your needs. If these modifications are crucial in a particular pipeline that you want others to reproduce (like in a research paper) our you are using that module over and over again, it might be an idea to prepare the package to be installed using `pip` directly from GitHub.

I learned to do it. I am collecting here some notes for future reference.

I followed the instructions provided in the [Python Packaging User Guide](https://packaging.python.org/tutorials/distributing-packages) from the beginning to section [Working in "Development Mode"](https://packaging.python.org/tutorials/distributing-packages/#working-in-development-mode) included.

I am using [`PyCQP_interface`](https://github.com/chozelinek/PyCQP_interface) as an example of a module prepared to be distributed over GitHub.

Basically:

1. Produce a README file describing the package.
1. Produce a LICENSE file.
1. Copy the file [`setup.py`](https://github.com/pypa/sampleproject/blob/master/setup.py)
1. Modify `setup.py`:
    1. line 17, since my README file is `README.md`;
    1. line 21, `PyCQP_interface`, the name of the module;
    1. line 26, `1.0.0`, choose the appropriate version;
    1. line 28, add a short description;
    1. line 32, add the URL to the GitHub repository;
    1. line 35, add name;
    1. line 36, add e-mail;
    1. line 39, license;
    1. line 63, keywords;
    1. line 77, required packages; and
    1. add line 88, `python_requires='>=3'` to indicate only compatible with Python 3.

Once you have `setup.py` ready, push to the repository and then install it in a local environment using:

```shell
pip install -e git+https://github.com/chozelinek/PyCQP_interface.git#egg=pycqp-interface
```

And that's it! If you have written a general purpose Python package, and you would like to officially share it with the Python community you should follow the instructions from the [Python Packaging Authority](https://www.pypa.io/en/latest/) to make it available through the [Python Package Index](https://pypi.python.org/pypi).
