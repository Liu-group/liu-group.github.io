---
layout: post
title: Tutorial 3. Basics of running TeraChem
---

### Learning objectives
* Run a simple GPU based quantum chemistry calculation
* Get familiar with running calculation on computer clusters

### Introduction
In previous tutorial we have tried running quantum chemistry calculation with CPU based program installed on your own computer. Here we will use TeraChem, the GPU based quantum chemistry package. The software is installed on the super computers with suitable GPU hardware, so we need to remote log in the super computers and run calculations there, as shown in the figure cited below:


<a href="https://upload.wikimedia.org/wikipedia/commons/4/40/Beowulf.png"><img src="https://upload.wikimedia.org/wikipedia/commons/4/40/Beowulf.png" alt="Schematic of typical architecture of a computer cluster."/></a>

**Figure 1**.Schematic of typical architecture of a computer cluster Available from: [https://en.wikipedia.org/wiki/Computer_cluster](https://en.wikipedia.org/wiki/Computer_cluster) [accessed 9 Sep, 2020]

Unlike your own computer, there are many users on the supercomputer. If all users directly run the calculation at the same time, the computer may freeze or even crash. Besides, supercomputers are clusters of a large number of computers connected in the network, and each computer is known as a **node**. The **node** where you log in may not have the hardware you need, and directly running the calculation in your home directory may result in failure. Instead, you need to tell the cluster you want the calculation to be done on the qualifying **nodes**.

Therefore, to run a calculation, we need to send the calculations to the **queue**. A program known as a **job scheduler** will schedule the calculations to start one by one, based on first come first serve or some other logics. Therefore, there is one extra step to run the calculation: write a bash script to let the queueing system know what calculation you need to run. Then send this script to the **job scheduler** to start waiting in the queue. This process is called *submitting a job*.
### Try and Learn

Here we will run a simple calculation with [TeraChem](http://www.petachem.com/products.html): geometry optimization of ethylene.

![ethylene]({{ site.url }}{{ site.baseurl }}/images/tutorial-3/ethylene.gif)

The task includes some review of preparing input files and read output files, and the new experience about *submitting jobs*. We will use the Xsede Comet GPU resources, assuming you have obtained an account.

(0) Log into the computing cluster.

You can find a detailed instruction on the [Xsede website](https://portal.xsede.org/sdsc-comet#access:login)

(1) Preparation of initial structure of ethylene molecule  *c2h4.xyz*

This can be most easily done by drawing or inserting fragments in [Avogadro](https://avogadro.cc/).

(2) Prepare the input file for TeraChem: *geoopt.in*

This can be done in a few different ways.
* [Avogadro](https://avogadro.cc/)  has an input file generation functionality
* Check [TeraChem UserGuide](http://www.petachem.com/doc/userguide.pdf) and follow the instructions there.
* Ask your colleagues for example input file and modify from there

(3) Prepare a jobscript.
This can be done in a few different ways.
* Check out the [documentation]( https://portal.xsede.org/sdsc-comet#access:login) of the computer cluster
* Ask your colleagues for example jobscript

(4) Read through the output files of your calculation and look for the optimized structure. Find the optimal bond lengths, angles, and dihedrals.
* Check the "geometry optimization" related sections in [TeraChem UserGuide](http://www.petachem.com/doc/userguide.pdf) and see whether the structure files are
