---
layout: post
comments: true
title: Tutorial 20. Interactive tutorial -- Making/reading scientific plots with less ambiguity 
---

### Learning objectives
* Recognize how the axis scale type, axis range, and other parameters can influence the appearance of a scientific plot.
* Tips for reading figures/ plotting figures with less ambiguity.

## Background

Making scientific plots and reading scientific plots are essential skills for chemistry students. It is important to realize that we should pay attention to some subtleties of a plot to avoid being misguided by its appearance.

For example, a student in my class found a paradox for the appearance of the blackbody radiation curve.

We have learned about plotting the blackbody radiation equation in an earlier tutorial ([Tutorial 11](/interactive-ultraviolet)).

We can either plot the energy density function $$\rho(\nu,T)$$ as a function of frequency $$\nu$$, or as a function of wavelength $$\lambda$$.

## Try ploting (with ambiguity)

Instruction:
* First click "Activate" to activate the code block
* Once you see the buttons to "run" at the bottom left corner of the code block, click "run" to run the code. Please be patient. Starting the kernel can be slow sometimes.
* You will see two plots for the Planck's radiation curves, as a function of $$\nu$$ or $$\lambda$$.

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.css" integrity="sha512-5A8nwdMOWrSz20fDsjczgUidUBR8liPYU+WymTZP1lmY9G6Oc7HlZv156XqnsgNUzTyMefFTcsFH/tnJE/+xBg==" crossorigin="anonymous" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.3.4/require.min.js"></script>

<script type="text/x-thebe-config">
  {
    requestKernel: true,
    binderOptions: {
      repo: "matplotlib/ipympl",
      ref: "0.6.1",
      repoProvider: "github",
    },
  }
</script>
<script src="https://unpkg.com/thebe@latest/lib/index.js"></script>

<button id="activateButton" style="width: 120px; height: 40px; font-size: 1.5em;">
  Activate
</button>
<script>
var bootstrapThebe = function() {
    thebelab.bootstrap();
}
document.querySelector("#activateButton").addEventListener('click', bootstrapThebe)
</script>

<pre data-executable="true" data-language="python">
{% highlight python %}
import numpy as np
import matplotlib.pyplot as plt

# A series of nu in the range from 1 Hz to 1 PHz (1e15 Hz), evenly distributed in the log scale
nu = np.logspace(np.log10(1),np.log10(1e15), num=1000, endpoint=True)
wavelength = np.linspace(100,10000, num=1000, endpoint=True)
h = 6.62607015e-34
c = 299792458
k = 1.380649e-23
nm2m = 1e-9

def rho_planck(nu,T):
    return 8*np.pi*h*(nu/c)**3/(np.exp(h*nu/(k*T))-1)

def RhoNu(T):
    plt.plot(nu, rho_planck(nu, T), label='Planck')
    # Add a title
    plt.title(r'$\rho(\nu;T)$ at $T$ = ' + str(T) + ' K')
    # Add X and y Label
    plt.xlabel(r'$\nu$ (Hz)')
    plt.ylabel(r'$\rho(\nu)$ (J$\times$m$^{-3}$)')

    # Set the x and y ranges to display
    plt.ylim(0, 2*max(rho_planck(nu, T)))
    plt.xlim(1e12,1e16)
    plt.xscale('log')
    plt.legend()

def RhoLambda(T):
    plt.plot(wavelength, rho_planck(c/(wavelength*nm2m), T), label='Planck')
    # Add a title
    plt.title(r'$\rho(\lambda;T)$ at $T$ = ' + str(T) + ' K')
    # Add X and y Label
    plt.xlabel(r'$\lambda$ (nm)')
    plt.ylabel(r'$\rho(\lambda)$ (J$\times$m$^{-3}$)')

    # Set the x and y ranges to display
    plt.ylim(0, 2*max(rho_planck(c/(wavelength*nm2m), T)))
    #print(max(rho_planck(c/(wavelength*nm2m), T)))
    plt.xlim(0,10000)
    plt.xscale('linear')
    plt.legend()
 
fig1=plt.figure()
RhoNu(298)   
fig2=plt.figure()
RhoLambda(5000)  
{% endhighlight %}
</pre>


## Paradox

Then here comes the paradox: We know and the relation between $$\nu$$ and $$\lambda$$ is $$\frac{c}{\mu} = \lambda$$.

Therefore, we expect that the $$\rho$$ curve should have different skews when we change the horizontal axis from $$\nu$$ to $$\lambda$$, i.e., the peak of the $$\nu$$ plot is on the left side, and one expects the peak of the $$\lambda$$ plot to be on the opposite side of the axis due to the reciprocal relation between $$\lambda$$ and $$\nu$$. However, we see that the peaks on both plots are on the left side of the horizontal axis. Why?

## Answer

The reason is that the two plots are not directly comparable.

1. They are plotted at different temperatures. 
   The $$\nu$$ plot is plotted for $T=298 K$. The $$\lambda$$ plot is plotted for $$T=5000k$$.
   Although we focus on the $$\rho$$ - $$\nu$$ relationship, there is another parameter controlling the $$\rho$$ function: the temperature. That information is given in the title of the plot.

2. They are plotted at different ranges of $$\nu$$.
   The $$\nu$$ plot is plotted for frequency in the range of 1 Hz to 1e15 Hz, as shown in the source code comments.
   The $$\lambda$$ plot is plotted for the wavelength range of 100 nm to 1000 nm, corresponding to the frequency in the range of 3e15 Hz to 3e13 Hz.
   The ranges of frequencies are not the same in the two plots.

3. The $$\nu$$ plot uses a log scale for the horizontal axis. The $$\lambda$$ plot uses a linear scale for the horizontal axis. This difference further changed the appearance of the curves, making it hard to compare.

## Take home messages

It's easy to get confused when reading plots, or confuse the readers when making plots. Ask yourself the following questions when reading/making a plot:
1. What is the range of the axes?
2. What scale are the axes? Linear or log scale?
3. Are there additional parameters that control the plot other than the variables shown on the axis?

When reading the plots in publications, carefully read the captions and axis labels to avoid misunderstanding the plot.

When making plots for your homework, or research projects, make sure that we provide all the information clearly to make the reader understand what you mean to present.
