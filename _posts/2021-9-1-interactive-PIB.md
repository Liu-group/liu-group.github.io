---
layout: post
comments: true
title: Tutorial 12. Interactive -- Particle in a box in 1D
---

### Learning objectives
* Try the interactive python code to plot the 1D particle in a box wave function.
* Observe the change of the wave function with different box lengths and quantum numbers.

## Background
1D particle in a box problem is almost always the first model system discussed in quantum chemistry courses. We want to solve the time-independent Schrödinger equation 

$$\hat{H}\psi = E\psi$$

for a 1-D particle. The particle moves in 1D, with a potential that models the walls of a box must not allow the particle to get out of the range $0\le x\le L$.

$$V(x) =
\begin{cases}
\infty & x<0 \textrm{ or } x>L\\
0 & 0 \le x \le L
\end{cases}
$$

### Solution
The solution of 1D particle in a box:
$$\psi(x)=
\begin{cases}
0 & x<0 \textrm{ or } x>L\\
\psi_n(x) = \sqrt{2/L}\textrm{sin}(\frac{n\pi x}{L}), \ \ n=1,2,3\dots & 0 \le x \le L
\end{cases}
$$

## Try and learn

Run the following code to see how the wave function shape changes with the quantum number $n$ and box length $L$ (both can be tuned interactively).

Instruction:
* First, click "Activate" to activate the code block.
* Once you see the buttons to "run" at the bottom left corner of the code block, click "run" to run the code.
* Please be patient. Starting the kernel can be slow sometimes.
* You will see a plot with two panels. Upper is $\psi$; lower is $\vert\psi\vert ^2$.
* Play with the slider to select the quantum number you want. $n$ can be any positive integer. In this demo, we allow $n$ up to 100.
* Enter the wanted box length (in atomic unit bohr) in the text box. In this demo, we allow $L$ to be any float number between 0.1 and 100 just for demonstration purposes. Theoretically, $L$ can be any positive number.


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

def psi(x,n,L):
    return np.sqrt(2/L)*np.sin(n*np.pi*x/L)

# Box length can be any float value between 0.1 and 100 inputed by the user
L_button = widgets.BoundedFloatText(
    description=r'$L$:',                             
    value=1.0,
    min=0.1,
    max=100.0,
    padding = 8)

# Quantum number can be any integer between 1 and 100
n_options = widgets.IntSlider(
    value=1,
    min=1,
    max=100,
    step=1,
    description=r'$n$:',
    disabled=False,
)

# Plot psi and |psi|**2 in two subplots
fig,(ax1,ax2) = plt.subplots(2, sharex=True)
plt.subplots_adjust(hspace=0)

@widgets.interact(L=L_button, n=n_options)
def update(n,L):
    ax1.cla()
    ax2.cla()

    x = np.linspace(0,L, num=1000)

    ###################
    # First work on psi
    ####################

    plt.sca(ax1)
    plt.plot(x,psi(x,n,L))
    # Add a title
    plt.title('Particle in a 1D box\n' + r'$L$='+str(L) + ' bohr, $n$=' + str(n))
    # Add y Label
    plt.ylabel(r'$\psi$')

    # Draw the walls of the box
    ymin = -np.sqrt(2/L)
    ymax = np.sqrt(2/L)
    margin = 0.1 #Allow 10% of margin for the x and y axis in both directions
    ylo = ymin*(1+margin) # lower boundary for y axis
    yup = ymax*(1+margin) # upper boundary for y axis
    #Left wall
    plt.plot([0,0],[ylo,yup],'gray', linewidth=5)
    #Right wall
    plt.plot([L,L],[ylo, yup],'gray',linewidth=5)

    # Set grid
    plt.grid(True)

    ########################
    # Then  work on |psi|**2  #
    ########################

    plt.sca(ax2)
    plt.plot(x,psi(x,n,L)**2,c='r')
    plt.fill_between(x,psi(x,n,L)**2, color='r',alpha=0.5)
    # Add X and y Label
    plt.xlabel(r'$x$ (bohr)')
    plt.ylabel(r'$|\psi|^2$')

    # Draw the walls of the box
    ymin = -2/L
    ymax = 2/L
    margin = 0.1 #Allow 10% of margin for the x and y axis in both directions
    ylo = ymin*(1+margin) # lower boundary for y axis
    yup = ymax*(1+margin) # upper boundary for y axis
    #Left wall
    plt.plot([0,0],[ylo,yup],'gray', linewidth=5)
    #Right wall
    plt.plot([L,L],[ylo, yup],'gray',linewidth=5)

    # Set the x and y ranges to display
    plt.ylim(ylo,yup)
    plt.xlim(-L*margin,L+L*margin)
    plt.grid(True)
{% endhighlight %}
</pre>

## Question
* The wavefunction is zero at certain points; these points are called nodes. How does the number of nodes change with $n$?
* Where is the maximum of the probability distribution when $n=1$?
* Where are the maxima of the probability distribution when $n$ takes large numbers, say 100? How does that relate to the result of classical mechanics?
