---
layout: post
title: Tutorial 2. Visualize molecular structures
---

### Learning objectives
* Learn to visualize molecular structures for molecular simulation.
* Topics:
    1. Common software for molecular structure visualization.
    1. Visualize a static molecule structure.
    1. Visualize a trajectory (a moving molecule).

## Prerequisites
1. Install the free molecule visualizers [Avogadro](https://avogadro.cc/) , [VMD](https://www.ks.uiuc.edu/Development/Download/download.cgi?PackageName=VMD)


## Visualize a static structure
In [Tuotorial 0]({{ site.url }}{{ site.baseurl }}/simulation-basics/#initxyz), we generated the initial structure for geometry optimization for water molecule in xyz format (the file is [here]({{ site.url }}{{ site.baseurl }}/simulation-basics/#initxyz)). How do we visualize the molecule?

For beginners of molecular simulation, we highly recommend using [Avogadro](https://avogadro.cc/) for visualizing static structures because of the simple interface and intuitive manipulation of the molecules. Specifically, you are encouraged to explore the following features of [Avogadro](https://avogadro.cc/)

* Measure bond lengths, angles, and diehedrals (for molecules with more atoms)

* Build molecules from scratch as if you are drawing in Paint Brush.

* Insert molecule fragments as starting point to build a new molecule

* Drag the molecule to change its structure (bond lengths, angles, dihedrals)

**Exercise:**:
* Substitute elements (change H$_2$O to SH$_2$)
* Drag the water molecule to to have both bonds at 1.0 Angstrom
* Generate the xyz file for a tolune molecule

## Visualize a moving structure
In [Tuotorial 1]({{ site.url }}{{ site.baseurl }}/python-basics/), we have a gif figure showing a water doing scissor vibration. How do we make a cool figure like that?

<img src="{{ site.url }}{{ site.baseurl }}/images/tutorial-01/water-forever.gif" alt="Alt text that describes the graphic" title="A vibrating water" style="width: 200px;" />

In [Tuotorial 1]({{ site.url }}{{ site.baseurl }}/python-basics/), you have already generated the water structures with different H-O-O angles, then the figure with vibrating water molecule can be generated from [VMD](https://www.ks.uiuc.edu/Development/Download/download.cgi?PackageName=VMD)

First get the water xyz files ready. No worries if you have not done [Tuotorial 1]({{ site.url }}{{ site.baseurl }}/python-basics/). You can download the xyz files [here]({{ site.url }}{{ site.baseurl }}/images/tutorial-2/xyzfiles.zip). Unzip the file.

Now in your terminal, type

{% highlight bash %}
vmd *.xyz
{% endhighlight %}

Then you will see the screen with a line that does not look like water at all.
![startVMD]({{ site.url }}{{ site.baseurl }}/images/tutorial-2/startvmd.png)

The red straight line with white ends is actually the water molecule at 180 degree. We can change the representation to make it look nicer.

First in the small control window (shown below), select the graphics menu and representations.
![startVMD]({{ site.url }}{{ site.baseurl }}/images/tutorial-2/graphics.png)

Click and you will see a new window like below

![startVMD]({{ site.url }}{{ site.baseurl }}/images/tutorial-2/representation.png)

Change the drawing method from *line* to *CPK*, then the structure will look like the following
![startVMD]({{ site.url }}{{ site.baseurl }}/images/tutorial-2/CPK.png)

Now it looks like a molecules. Click the play button on the control window to play the movie and watch the molecule move!

![startVMD]({{ site.url }}{{ site.baseurl }}/images/tutorial-2/play_movie.png)

**Exercise:** Can you save the movie into a gif graph or a movie file? Search for VMD manual for the answer.
