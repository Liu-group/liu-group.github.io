---
layout: post
comments: true
title: Tutorial 11. Interactive tutorial -- Ultraviolet Catastrophe
---

### Learning objectives
* Try the interactive python code to plot the blackbody radiation curve with classical and Planck's formula.
* Learn the history of "ultraviolet catastrophe" and the birth of quantum mechanics

## Background

*"The beauty and clearness of the dynamical theory, which asserts heat and light to be modes of motion, is at present obscured by two clouds"* -- Lord Kelvin

Kelvin explained in his speech in 1900 that the "clouds" were two unexplained phenomena: the inability to detect the luminous ether, and the blackbody radiation.

### How should the blackbody radiation intensity look like as a function of the temperature of the blackbody?

* According to classical physics, an idealized blackbody at temperature $T$ emits radiation with a given energy density $\rho(\nu,T)$, where $\nu$ is the frequency of the light emitted.

$$ \rho(\nu;T)_{\textrm{classical}}d\nu = \frac{8\pi k_B T}{c^3}\nu^2d\nu $$

* In 1900, Max Planck assumed that light could be describe as a set of harmonic oscillators, and that each oscillator carried only a discrete amount of energy

$$ E=nh\nu $$

where $n = 0, 1, 2$ is a non-negative integer and $h$ is Planck’s constant. Then the blackbody radiation has the following formula

$$\rho(\nu;T)_{\textrm{Planck}}=\frac{8\pi h}{c^3}\frac{\nu^3 d\nu}{e^{h\nu/k_B T}}$$


## Try and learn

Run the following code to compare and contrast the energy density of blackbody radiation as a function of the frequency of the light emitted. Temperature (T) can be tuned interactively with the control bar.

Instruction:
* First click "Activate" to activate the code block
* Once you see the buttons to "run" at the bottom left corner of the code block, click "run" to run the code. Please be patient. Starting the kernel can be slow sometimes.
* You will see a plot with the radiation curves.
* Play with the control bar to change the temperature.
* You can modify the code and plot $\rho(T)$ as a function of the wavelength of the light.

*Note*: The code block uses `Numpy`, `matplotlib`, and `Ipywidgets` packages. I didn't include Scipy because it slowed down the initilization of the executable code blocks based on my tests. Therefore, the physical constants are manually defined, instead of directly using `scipy.constants`.


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
%matplotlib widget
import ipywidgets as widgets
import numpy as np
import matplotlib.pyplot as plt

T = 298 # 25 degrees c

# A series of nu in the range from 1 Hz to 1 PHz (1e15 Hz), evenly distributed in the log scale
nu = np.logspace(np.log10(1),np.log10(1e15), num=1000, endpoint=True)
h = 6.62607015e-34
c = 299792458
k = 1.380649e-23

def rho_classical(nu,T):
    return 8*np.pi*k*T/(c**3)*(nu**2)

def rho_planck(nu,T):
    return 8*np.pi*h*(nu/c)**3/(np.exp(h*nu/(k*T))-1)

@widgets.interact(T=(98,3000, 100))
def update(T = 298):
    plt.clf()
    plt.plot(nu, rho_classical(nu, T), label='classical')
    plt.plot(nu, rho_planck(nu, T), label='Planck')
    # Add a title
    plt.title(r'$\rho(T)$ at $T$ = ' + str(T) + ' K')
    # Add X and y Label
    plt.xlabel(r'$\nu$ (Hz)')
    plt.ylabel(r'$\rho(\nu)$ (J$\times$m$^{-3}$)')

    # Set the x and y ranges to display
    plt.ylim(0, 2*max(rho_planck(nu, T)))
    plt.xlim(1e12,1e16)
    plt.xscale('log')
    plt.legend()
{% endhighlight %}
</pre>

## Further Reading
Ultraviolet Catastrophe on [Wikipedia](https://en.wikipedia.org/wiki/Ultraviolet_catastrophe)
