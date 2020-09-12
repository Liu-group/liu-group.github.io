---
layout: post
title: Tutorial 4. Jupyter notebook for computational chemistry
---

### Learning objectives
* Get familiar with different ways to run python
* Set up your personal computer ready to run Jupyter Notebook
* Learn to run Jupyter Notebook from remote cluster through port forwarding

### Introduction
Python is useful for computational chemistry in multiple aspects. In [Tuotorial 1]({{ site.url }}{{ site.baseurl }}/python-basics), we have demonstrated how we can use python to generate input files and analyze output files. Python can also be used for big data analysis, machine learning, and generating publication-quality figures.

Common ways to run python include:

| Run python in ... | Use case |
|-------|--------|
| command line| building a package and run on remote computer clusters |
| interactive session|  simple tasks like: using python as a calculator; checking whether some packages can be loaded|
| Jupyter Notebook|  integrate different usages of Python together in one place|


For the first two options, please refer to [this tutorial](https://realpython.com/run-python-scripts/). We will focus on the Jupyter Notebook.

### Try and Learn

Here we will demonstrate a few different ways to run Jupyter Notebook.

## 1. Use Readily available online service.
For example, from the XSEDE computational chemistry website: [https://chemcompute.org/jupyterhub](https://chemcompute.org/jupyterhub). There are many good examples of using Jupyter Notebooks for computational chemistry on that website. Please check them out.

**Pro**: No need to set up your own computer environment. Good for beginners.

**Con**: Need to upload your files. May not be convenient enough for daily code development.

## 2. Set up your own computer to run Jupyter Notebook.
Please follow the guidance of this [website](https://www.dataquest.io/blog/jupyter-notebook-tutorial/)

**Con**: More complicated set up

**Pro**: You can customize the conda environment freely to meet your development needs.

**Special Tips:**: To be able to switch between different conda environments in your Jupyter Notebook, you need to install `nb_conda`, `nb_conda_kernels` in your conda environment, as descripbed [here](https://stackoverflow.com/questions/39604271/conda-environments-not-showing-up-in-jupyter-notebook).

## 3. Use **port forwarding** to Run a jupyter notebook from remote computer cluster.
Detailed instructions can be found from many online resources, such as [this website](https://fizzylogic.nl/2017/11/06/edit-jupyter-notebooks-over-ssh/).

**Con**: Even more complicated set up

**Pro**: Some operations needs to be done in this way. For example, you have a very big dataset (millions of entries) on the remote cluster. Running on your own computer may be unrealistic, because
1. Your computer don't have a large enough disk space to copy over the dataset
2. You computer don't have enough memory to load the dataset, or loading it without being extremely slow or crash.
3. You would like to submit computing jobs to the queue through Jupyter notebook.
